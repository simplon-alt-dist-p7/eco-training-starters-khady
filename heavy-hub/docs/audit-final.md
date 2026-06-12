# Audit final - heavy-hub

---

## 1. Synthèse exécutive - comparaison avant / après


| Indicateur                                 | Audit initial (avant) | Audit final (cible)       | Audit final (mesuré) | Écart       |
| ------------------------------------------ | --------------------- | ------------------------- | -------------------- | ----------- |
| **Requêtes API au 1er chargement `/`**     | **5**                 | **≤ 2**                   | *À mesurer*          | −40 % min.  |
| **Polling notifications (idle)**           | **1 / 7 s**           | **0**                     | *À mesurer*          | −100 % idle |
| **Snapshot localStorage**                  | Présent (complet)     | **0 Ko** ou cache minimal | *À mesurer*          |             |
| **Appels `/api/notifications` / min idle** | **8–9**               | **0**                     | *À mesurer*          |             |


---

## 3. EcoIndex - comparaison


| Page             | EcoIndex AVANT | EcoIndex APRÈS (cible) | EcoIndex APRÈS (mesuré) | Eau AVANT | Eau APRÈS | GES AVANT | GES APRÈS |
| ---------------- | -------------- | ---------------------- | ----------------------- | --------- | --------- | --------- | --------- |
| `/`              | 78.41          | ≥ +10 pts              |                         | 2.15      |           | 1.43      |           |
| `/library`       | 77.00          | ≥ +10 pts              |                         | 2.19      |           | 1.46      |           |
| `/profile`       | 77.23          | stable ou ↑            |                         | 2.18      |           | 1.46      |           |
| `/notifications` | 75.00          | stable ou ↑            |                         | 2.25      |           | 1.50      |           |


---

## 4. Résultats attendus par user story

### US-1 - APIs à la demande


| Métrique                             | Avant    | Cible après            |
| ------------------------------------ | -------- | ---------------------- |
| Requêtes `/api/`* au chargement `/`  | 5        | ≤ 2                    |
| Endpoints chargés sur `/` uniquement | 5 (tous) | home + dashboard (ex.) |


---

### US-2 - Allègement pages / images


| Métrique                         | Avant      | Cible après        |
| -------------------------------- | ---------- | ------------------ |
| Poids images visibles `/library` | *baseline* | −30 % minimum      |
| Images hors viewport chargées    | Oui        | Non (lazy loading) |


---

### US-3 - Parcours plus court


| Métrique                                  | Avant      | Cible après         |
| ----------------------------------------- | ---------- | ------------------- |
| Clics bibliothèque → fiche profil/contenu | *baseline* | ≤ 3                 |
| Requêtes HTTP sur le parcours             | *baseline* | réduction mesurable |


---

### US-4 - Stockage local (optionnel M3)


| Métrique              | Avant         | Cible après                |
| --------------------- | ------------- | -------------------------- |
| Taille `hub-snapshot` | *baseline Ko* | 0 Ko ou préférences seules |


---

### US-5 - Notifications sobres (optionnel M4)


| Métrique                                       | Avant       | Cible après                     |
| ---------------------------------------------- | ----------- | ------------------------------- |
| Appels `/api/notifications` / min (idle 5 min) | 8–9         | 0                               |
| Refresh                                        | Polling 7 s | À l'ouverture + action manuelle |


---

## 5. Bilan qualitatif (à rédiger après mesure)

### Gains environnementaux observés

*À compléter :*

- Réseau :
- Terminal :
- Serveur :

### Limites / actions restantes (roadmap M5–M6)


| Action                             | Horizon | Lien ACV                                 |
| ---------------------------------- | ------- | ---------------------------------------- |
| Purge automatique demandes / logs  | M5      | Supprimer données inutiles               |
| Cache HTTP + hébergeur français    | M6      | Processus & exploitation                 |
| Surveillance performances continue | M6      | Surveiller pages lourdes et API inutiles |


---



