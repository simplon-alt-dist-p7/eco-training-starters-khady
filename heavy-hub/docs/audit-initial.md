# Audit initial - heavy-hub

**Date** : 11/06/2026 - statut : avant implémentation du backlog.

---

## 1. Synthèse exécutive (état actuel)


| Indicateur                             | Valeur baseline   | Commentaire                                      |
| -------------------------------------- | ----------------- | ------------------------------------------------ |
| **Requêtes API au 1er chargement `/`** | **5**             | home, library, dashboard, notifications, profile |
| **Polling notifications (idle)**       | **1 appel / 7 s** | `setInterval(refresh, 7000)`                     |
| **Snapshot localStorage**              | **Présent**       | Clé `hub-snapshot` - payloads complets           |
| **Cache HTTP serveur**                 | **Désactivé**     | `Cache-Control: no-store`, assets `maxAge: 0`    |
| **Lazy loading images**                | **Non**           | Toutes les `<img>` chargées immédiatement        |
| **Poids assets repo**                  | **40,5 Ko**       | 4 fichiers (`npm run analyze`)                   |
| **Poids data repo**                    | **18,5 Ko**       | 2 fichiers JSON                                  |


---

## 3. EcoIndex


| Page              | URL              | EcoIndex | Eau (cl) | GES (g CO₂e) | DOM | Poids page (Ko) | Nb requêtes | Date       |
| ----------------- | ---------------- | -------- | -------- | ------------ | --- | --------------- | ----------- | ---------- |
| Accueil connectée | `/`              | 78.41    | 2.15     | 1.43         | 188 | 1761            | 27          | 11/06/2026 |
| Bibliothèque      | `/library`       | 77.00    | 2.19     | 1.46         | 117 | 1793            | 30          | 11/06/2026 |
| Profil            | `/profile`       | 77.23    | 2.18     | 1.46         | 155 | 1836            | 34          |            |
| Messages          | `/notifications` | 75.00    | 2.25     | 1.50         | 130 | 1836            | 34          |            |


---

## 4. Mesures techniques détaillées

### 4.1 Appels API - chargement global (`HubApp.tsx`)

Au montage de l'application, **5 requêtes parallèles** sont déclenchées systématiquement :


| Endpoint                 | Rôle sur heavy-hub       |
| ------------------------ | ------------------------ |
| `GET /api/home`          | Accueil connecté         |
| `GET /api/library`       | Bibliothèque de contenus |
| `GET /api/dashboard`     | Suivi utilisateur        |
| `GET /api/notifications` | Fil de messages          |
| `GET /api/profile`       | Profil membre            |


### 4.2 Stockage local


| Clé            | Contenu                               | Impact                              |
| -------------- | ------------------------------------- | ----------------------------------- |
| `hub-snapshot` | Sérialisation JSON des 5 payloads API | Terminal - mémoire / disque inutile |


### 4.3 Page Messages (`/notifications`)


| Comportement               | Valeur             | Impact                                       |
| -------------------------- | ------------------ | -------------------------------------------- |
| Polling automatique        | Toutes les **7 s** | Réseau + serveur en continu                  |
| Appels API / minute (idle) | **8–9**            | Équivalent sollicitation mentors sans filtre |


### 4.4 Médias et pages


| Écran heavy-hub | Comportement observé                               |
| --------------- | -------------------------------------------------- |
| `/library`      | Grille de cartes - chaque carte charge `heroAsset` |
| `/`             | Cartes featured + images hero                      |
| `/profile`      | Avatar + cartes recommandées                       |
| `/content/:id`  | Hero + contenus liés                               |



| Ressource               | Taille repo |
| ----------------------- | ----------- |
| Assets (4 fichiers SVG) | 40,5 Ko     |
| Data JSON               | 18,5 Ko     |


---

### 4.5 Backend - absence de cache


| Paramètre        | Valeur actuelle                   |
| ---------------- | --------------------------------- |
| `Cache-Control`  | `no-store` (toutes réponses)      |
| Assets statiques | `maxAge: 0`                       |
| Relecture disque | `readJson()` à chaque requête API |


---

## 5. Anti-patterns identifiés


| Anti-pattern                             | Présent | Fichier / zone                            |
| ---------------------------------------- | ------- | ----------------------------------------- |
| Médias chargés sans lazy loading         | Oui     | `ContentCard`                             |
| Contenus préchargés inutilement          | Oui     | `HubApp` - 5 APIs au boot                 |
| Notifications bavardes                   | Oui     | `NotificationsPage` - polling 7 s         |
| Prefetch / appels inutiles au chargement | Oui     | `Promise.all` global                      |
| Stockage local superflu                  | Oui     | `localStorage` snapshot                   |
| Pas de cache HTTP                        | Oui     | `backend/src/index.ts`                    |
| Duplication composants                   | Partiel | `ContentCard` répété sur plusieurs écrans |


---

## 6. Parcours utilisateur baseline


| Parcours                 | Étapes                              | Clics | Requêtes API (estim.)             |
| ------------------------ | ----------------------------------- | ----- | --------------------------------- |
| Accueil seul             | Ouvrir `/`                          | 0     | **5** (toutes routes préchargées) |
| Annuaire → fiche         | `/` → Bibliothèque → Ouvrir contenu | 2+    | 5 + 1 (`/api/content/:id`)        |
| Consulter messages 5 min | `/notifications`                    | 1     | 5 init + **~40** polling          |


---

## 7. Pistes ACV testables sur heavy-hub


| Piste / bonne pratique MentorPromo | Observable sur heavy-hub (baseline)      |
| ---------------------------------- | ---------------------------------------- |
| Réduire requêtes HTTP par page     | 5 APIs au boot                           |
| Réduire poids des pages            | Images sans lazy loading                 |
| Parcours court                     | Parcours multi-clics documenté section 6 |
| Supprimer données inutiles         | Snapshot localStorage                    |
| Surveiller appels API inutiles     | Polling notifications 7 s                |


---

## 8. Commandes de mesure

```bash
npm install
npm run dev
npm run analyze
npm run lighthouse   
```

