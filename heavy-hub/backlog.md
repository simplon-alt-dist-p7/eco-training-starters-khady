# Backlog heavy-hub

## Contexte du projet

**Deux projets distincts :**


| Projet          | Rôle                                                                                                  |
| --------------- | ----------------------------------------------------------------------------------------------------- |
| **MentorPromo** | Service réel - ACV Flash et **méthodologie** d'éco-conception (`docs/acv-flash-khady.md`)             |
| **heavy-hub**   | Repo starter - **terrain d'expérimentation** pour appliquer cette méthodologie et mesurer avant/après |


**Références** : `docs/cartographie-cadrage.md`, [Slides plan d'action 6 mois](https://docs.google.com/presentation/d/1kQ9gD2V6eLYpNfeGysgCXbchujL-qUOb/edit?usp=sharing), `docs/audit-initial.md`, `docs/audit-final.md`, [Analyse EcoIndex (Excel)](https://docs.google.com/spreadsheets/d/1u0nX10ybVjA_yS0mieFsL0OivxkbP2KK/edit?usp=sharing), [ACV MentorPromo (Miro)](https://miro.com/welcomeonboard/azBKaFlpR1RqR1hRc2FBT0lnbUlKb3hIdUNuWTVmbmgvckpIdGd0SlU0VFEwNCthNXRLa09JWjNuTERxNEJXNHBzajYvVmFMaE9HOVJQdnBDZWlnd1JON1UyOGU2STZxYWdmcy9NTVJWQ0gyREt4bEFyRXRsYlR1TTB6QXNMMFd3VHhHVHd5UWtSM1BidUtUYmxycDRnPT0hdjE=?share_link_id=643304053230), [Bonnes pratiques (Excel)](https://docs.google.com/spreadsheets/d/1WRLaJbdfu5FhS5ES2Kp1FJ6S3ScEpGz2/edit?usp=sharing)

**3 pistes éco-conception de mon ACV transposées :**

1. Réduire le poids des pages (design, images, JS)
2. Supprimer automatiquement les données inutiles (demandes, logs)
3. Rendre le parcours utilisateur plus court (filtres, moins de pages inutiles)

---

## Traçabilité ACV → user stories

| Piste ACV MentorPromo | Côté navigateur (quick wins, M2-M3) | Côté serveur / structure (M4-M6) |
| --------------------- | ----------------------------------- | -------------------------------- |
| **1. Poids des pages** | US-1 appels API à la demande · US-2 images | US-6 pagination (DOM) · US-9 découpage JS |
| **2. Données inutiles** | US-4 stockage local · US-5 polling | US-8 purge automatique notifications + logs |
| **3. Parcours court** | US-3 parcours bibliothèque → fiche | US-6 pagination + filtres |
| *Processus & exploitation* (ACV, section 4 « Que puis-je améliorer ») | - | US-7 cache HTTP |

## Vue d'ensemble

| US | Titre | Piste | Priorité | Effort | Mois | Statut |
| -- | ----- | ----- | -------- | ------ | ---- | ------ |
| US-1 | Appels API à la demande | 1 | Haute | S | M2 |  Réalisé |
| US-5 | Notifications sans polling | 2 | Haute | S | M2 |  Réalisé |
| US-2 | Images allégées | 1 | Haute | S | M3 |  Réalisé |
| US-4 | Stockage local superflu | 2 | Moyenne | XS | M3 |  Réalisé |
| US-3 | Parcours bibliothèque → fiche | 3 | Moyenne | S | M3 |  Réalisé |
| US-6 | Pagination de la bibliothèque | 1 + 3 | Haute | M | M4 | À faire |
| US-7 | Cache HTTP | Exploitation | Haute | S | M5 | À faire |
| US-8 | Purge automatique des données | 2 | Haute | M | M5 | À faire |
| US-9 | Découpage du JS par route | 1 | Moyenne | M | M6 | À faire |

*Effort : XS < 1 h · S ≈ ½ jour · M ≈ 1-2 jours. Mesures : [Analyse EcoIndex (Excel)](https://docs.google.com/spreadsheets/d/1u0nX10ybVjA_yS0mieFsL0OivxkbP2KK/edit?usp=sharing).*

---

## User story 1 - Réduire les appels API au chargement

- **Contexte** : En tant qu'**utilisateur du portail**, je veux **accéder à l'accueil heavy-hub**, afin de **consulter mon espace sans déclencher des chargements API inutiles**.
- **Objectif** : passer de **5 appels API** au démarrage à **≤ 2** sur la page d'accueil (`/`).
- **Bonne pratique d'éco-conception ciblée** : *Réduire le nombre de requêtes HTTP par page* (ACV, section 4 « Que puis-je améliorer » - design & développement, piste n°1).
- **KPI associé** : nombre de requêtes `/api/*` au 1er chargement de `/` (DevTools → Network).
- **Repo ou écran concerné** : heavy-hub - `frontend/src/HubApp.tsx` (`useEffect` avec `Promise.all`), écran `/`.
- **Critère de réussite** : sur `/`, Network affiche ≤ 2 requêtes API avant interaction ; les autres données sont chargées à l'ouverture de leur écran.
- **Niveau de priorité** : **haute**
- **Résultat** :  **5 → 2** (−60 %) · requêtes totales de `/` : 12 → 9.

**Transfert MentorPromo** : charger l'annuaire des mentors seulement à l'ouverture de l'annuaire, pas dès la connexion.

---

## User story 2 - Alléger le poids des images

- **Contexte** : En tant qu'**utilisateur**, je veux **parcourir la bibliothèque heavy-hub**, afin de **consulter des contenus sans télécharger des vignettes plus lourdes que nécessaire**.
- **Objectif** : réduire de **≥ 30 %** le poids des images transférées à l'ouverture de `/library`.
- **Bonne pratique d'éco-conception ciblée** : *Optimiser les images - compresser et ne charger que ce qui est visible* (ACV piste n°1).
- **KPI associé** : poids des images transférées (Ko) à l'ouverture de `/library`, sans défilement.
- **Repo ou écran concerné** : heavy-hub - `assets/*.svg` (compression) et composant `ContentCard` (`loading="lazy"`), écran `/library`.
- **Critère de réussite** : baisse ≥ 30 % du KPI, mesurée avec le même protocole avant et après.
- **Niveau de priorité** : **haute**
- **Résultat** :  **30,9 → 6,7 Ko (−78 %)** · poids total de `/library` : 240 → 213 Ko (−11 %).

**Note** : les 9 cartes réutilisent 3 fichiers SVG, le gain vient donc surtout de la compression. Le lazy loading deviendra utile avec de vraies vignettes distinctes (cas de MentorPromo : photos de profil des mentors).

---

## User story 3 - Parcours plus court bibliothèque → fiche

- **Contexte** : En tant qu'**utilisateur**, je veux **aller de l'accueil à une fiche contenu en filtrant la bibliothèque**, afin de **trouver l'information sans consulter ni charger d'écrans inutiles**.
- **Objectif** : réduire le nombre de requêtes API du parcours Accueil → Bibliothèque → Fiche, en gardant **≤ 3 clics**.
- **Bonne pratique d'éco-conception ciblée** : *Favoriser un usage sobre - parcours court, peu de clics* (ACV piste n°3).
- **KPI associé** : nombre de requêtes `/api/*` sur le parcours Accueil → Bibliothèque → Fiche (+ nombre de clics).
- **Repo ou écran concerné** : heavy-hub - `LibraryPage`, `ContentPage`, routes `/library` et `/content/:id`.
- **Critère de réussite** : moins de requêtes API que la baseline sur le même parcours ; nombre de clics ≤ 3.
- **Niveau de priorité** : **moyenne** (le parcours ne comptait déjà que 2 clics ; le gain porte sur les requêtes)
- **Résultat** :  **6 → 4 requêtes API** · 2 clics (Bibliothèque, puis Ouvrir), filtres format/niveau sans rechargement.

---

## User story 4 - Supprimer le stockage local superflu

- **Contexte** : En tant qu'**utilisateur**, je veux **un portail qui ne stocke pas de copies inutiles de mes données**, afin de **ne pas encombrer la mémoire de mon terminal**.
- **Objectif** : ramener le snapshot `hub-snapshot` à **0 Ko**.
- **Bonne pratique d'éco-conception ciblée** : *Limiter le stockage local aux données strictement nécessaires* (ACV piste n°2, côté navigateur).
- **KPI associé** : taille de `localStorage['hub-snapshot']` (Ko) après visite des 6 écrans.
- **Repo ou écran concerné** : heavy-hub - `HubApp.tsx` (`localStorage.setItem`).
- **Critère de réussite** : aucune copie des réponses API dans le localStorage.
- **Niveau de priorité** : **moyenne**
- **Résultat** :  **0 Ko** (clé supprimée, localStorage vide).

---

## User story 5 - Notifications sobres (sans polling permanent)

- **Contexte** : En tant qu'**utilisateur**, je veux **consulter mes messages**, afin de **ne pas solliciter le réseau en permanence quand la page reste ouverte sans action**.
- **Objectif** : supprimer le polling toutes les **7 s** ; rafraîchir à l'ouverture ou à la demande.
- **Bonne pratique d'éco-conception ciblée** : *Pas d'appels répétés sans besoin* (ACV, section 4 « Que puis-je améliorer » - surveiller les appels API inutiles, piste n°2).
- **KPI associé** : appels `/api/notifications` sur 5 min d'inactivité (cible : **0**).
- **Repo ou écran concerné** : heavy-hub - `NotificationsPage` (`setInterval(refresh, 7000)`), écran `/notifications`.
- **Critère de réussite** : 0 appel API pendant 5 min sans interaction ; bouton « Actualiser » fonctionnel.
- **Niveau de priorité** : **haute**
- **Résultat** :  **~40 → 0 appel** sur 5 min.

**Transfert MentorPromo** : même logique pour les demandes de contact (pas de rafraîchissement automatique, notification par email).

---

## User story 6 - Paginer la bibliothèque

- **Contexte** : En tant qu'**utilisateur**, je veux **voir les contenus de la bibliothèque par pages de 6 et les filtrer côté serveur**, afin de **ne charger que les contenus utiles à ma recherche**.
- **Objectif** : réduire le DOM de `/library` de **≥ 30 %** et gagner **≥ 2 points EcoIndex** sur cet écran.
- **Bonne pratique d'éco-conception ciblée** : *Limiter le nombre d'éléments affichés et filtrer à la source* (ACV pistes n°1 et n°3).
- **KPI associé** : nombre d'éléments DOM et EcoIndex de `/library`.
- **Repo ou écran concerné** : heavy-hub - `LibraryPage` + `GET /api/library?format=&level=&page=` (`backend/src/index.ts`).
- **Critère de réussite** : DOM `/library` ≤ 107 (baseline 153) ; EcoIndex `/library` ≥ 90,1 (baseline 88,1).
- **Niveau de priorité** : **haute** - c'est le DOM (coefficient ×3) qui pèse le plus sur l'EcoIndex.

**Transfert MentorPromo** : annuaire des mentors paginé et filtré par domaine / promo côté serveur.

---

## User story 7 - Mettre en cache les réponses HTTP

- **Contexte** : En tant qu'**utilisateur régulier** (1 session / semaine), je veux **que les fichiers déjà téléchargés soient réutilisés**, afin de **ne pas retélécharger la même chose à chaque visite**.
- **Objectif** : réduire de **≥ 50 %** les octets transférés lors d'une 2e visite de `/`.
- **Bonne pratique d'éco-conception ciblée** : *Utiliser le cache HTTP* (ACV, section 4 « Que puis-je améliorer » - processus & exploitation).
- **KPI associé** : poids transféré (Ko) d'une 2e visite de `/` avec le cache navigateur actif.
- **Repo ou écran concerné** : heavy-hub - `backend/src/index.ts` (`Cache-Control: no-store`, `maxAge: 0`).
- **Critère de réussite** : assets servis avec `Cache-Control: max-age` long ; API avec `ETag` ; KPI ≥ −50 %.
- **Niveau de priorité** : **haute**

---

## User story 8 - Purger automatiquement les données inutiles

- **Contexte** : En tant qu'**administrateur du service**, je veux **que les anciennes notifications lues et les logs de plus de 30 jours soient supprimés automatiquement**, afin de **ne pas stocker ni transférer de données qui ne servent plus**.
- **Objectif** : `/api/notifications` ne renvoie que les notifications non lues ou de moins de 30 jours ; logs limités à 30 jours.
- **Bonne pratique d'éco-conception ciblée** : *Supprimer automatiquement les données inutiles - politique de rétention* (ACV piste n°2, côté serveur, RGPD).
- **KPI associé** : nombre d'éléments et poids (Ko) de la réponse `/api/notifications` ; volume des logs serveur.
- **Repo ou écran concerné** : heavy-hub - `backend/src/index.ts` (`/api/notifications`, middleware de log), `data/hub.json`.
- **Critère de réussite** : réponse `/api/notifications` réduite d'au moins 30 % ; tâche de purge documentée et testée.
- **Niveau de priorité** : **haute** - c'est la piste n°2 de l'ACV, pas encore couverte côté serveur.

**Transfert MentorPromo** : purge des demandes de contact traitées et des comptes inactifs (piste n°2 + RGPD).

---

## User story 9 - Découper le JavaScript par route

- **Contexte** : En tant qu'**utilisateur sur un terminal ancien**, je veux **ne télécharger que le code de l'écran que j'ouvre**, afin de **réduire le temps de chargement et l'énergie consommée par mon appareil**.
- **Objectif** : réduire de **≥ 30 %** le JS chargé sur `/`.
- **Bonne pratique d'éco-conception ciblée** : *Réduire la taille du JavaScript chargé* (ACV, section 4 « Que puis-je améliorer » - design & développement, piste n°1).
- **KPI associé** : poids du JS (Ko) transféré au 1er chargement de `/`.
- **Repo ou écran concerné** : heavy-hub - `frontend/src/App.tsx` / `HubApp.tsx` (`React.lazy` par route).
- **Critère de réussite** : JS initial de `/` réduit de ≥ 30 % (avant : un seul fichier JS de 183 Ko).
- **Niveau de priorité** : **moyenne**
