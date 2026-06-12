# Backlog heavy-hub

## Contexte du projet

**Deux projets distincts :**


| Projet          | Rôle                                                                                                  |
| --------------- | ----------------------------------------------------------------------------------------------------- |
| **MentorPromo** | Service réel - ACV Flash et **méthodologie** d'éco-conception (`docs/acv-flash-khady.md`)             |
| **heavy-hub**   | Repo starter - **terrain d'expérimentation** pour appliquer cette méthodologie et mesurer avant/après |


**Références** : `docs/cartographie-cadrage.md`, `docs/audit-initial.md`, `docs/audit-final.md`

**3 pistes éco-conception de mon ACV transposées :**

1. Réduire le poids des pages (design, images, JS)
2. Supprimer automatiquement les données inutiles (demandes, logs)
3. Rendre le parcours utilisateur plus court (filtres, moins de pages inutiles)

---

## User story 1 - Réduire les appels API au chargement

- **Contexte** : En tant qu'**utilisateur du portail**, je veux **accéder à l'accueil heavy-hub**, afin de **consulter mon espace sans déclencher des chargements API inutiles**.
- **Objectif** : passer de **5 appels API** au démarrage à **≤ 2** sur la page d'accueil (`/`).
- **Bonne pratique d'éco-conception ciblée** : *Réduire le nombre de requêtes HTTP par page* (piste MentorPromo - design & développement).
- **KPI associé** : nombre de requêtes `/api/`* au 1er chargement de `/` (DevTools → Network).
- **Repo ou écran concerné** : `frontend/src/HubApp.tsx` - `useEffect` avec `Promise.all`.
- **Critère de réussite** : sur `/`, Network affiche ≤ 2 requêtes API avant interaction ; gain documenté dans `audit-final.md`.
- **Niveau de priorité** : **haute**

**Justification méthodologique MentorPromo** : même levier que « réduire les requêtes HTTP » identifié sur l'annuaire et les appels API back-end.

---

## User story 2 - Alléger le poids des pages (images et médias)

- **Contexte** : En tant qu'**utilisateur**, je veux **parcourir la bibliothèque heavy-hub**, afin de **consulter des contenus sans télécharger toutes les vignettes d'un coup**.
- **Objectif** : réduire de **≥ 30 %** le poids des images au scroll initial de `/library`.
- **Bonne pratique d'éco-conception ciblée** : *Optimiser les images et scripts - réduire le poids chargé côté navigateur* (piste MentorPromo n°1).
- **KPI associé** : poids transféré images (Ko) sur les 3 premières cartes visibles à l'ouverture de `/library`.
- **Repo ou écran concerné** : `frontend/src/HubApp.tsx` - composant `ContentCard`.
- **Critère de réussite** : images hors viewport absentes de Network jusqu'au scroll ; baisse mesurable dans l'audit final.
- **Niveau de priorité** : **haute**

**Justification méthodologique MentorPromo** : transposition directe de « réduire le poids des pages ».

---

## User story 3 - Parcours plus court (filtres et navigation)

- **Contexte** : En tant qu'**utilisateur**, je veux **filtrer la bibliothèque et accéder à une fiche**, afin de **limiter les écrans consultés inutilement**.
- **Objectif** : atteindre une fiche contenu en **≤ 3 clics** depuis `/library`.
- **Bonne pratique d'éco-conception ciblée** : *Favoriser un usage sobre - parcours court, peu de clics* (piste MentorPromo n°3).
- **KPI associé** : nombre de pages vues + requêtes HTTP sur le parcours bibliothèque → fiche.
- **Repo ou écran concerné** : `LibraryPage`, routes `/library`, `/content/:id`.
- **Critère de réussite** : parcours documenté en ≤ 3 clics ; réduction vs baseline `audit-initial.md`.
- **Niveau de priorité** : **haute**

**Justification méthodologique MentorPromo** : transposition de « recherche plus précise, meilleurs filtres, moins de pages inutiles ».

---

## User story 4 - Supprimer le stockage local superflu

- **Contexte** : En tant qu'**utilisateur**, je veux **un portail réactif**, afin de **ne pas saturer la mémoire du terminal**.
- **Objectif** : supprimer ou réduire le snapshot `hub-snapshot` à **0 Ko** de données dupliquées.
- **Bonne pratique d'éco-conception ciblée** : *Limiter le stockage local aux données strictement nécessaires* (piste MentorPromo n°2 - purge données).
- **KPI associé** : taille `localStorage['hub-snapshot']` (Ko) après 1ère visite.
- **Repo ou écran concerné** : `HubApp.tsx` - `localStorage.setItem`.
- **Critère de réussite** : pas de duplication complète des 5 payloads API en localStorage.
- **Niveau de priorité** : **moyenne**

**Justification méthodologique MentorPromo** : même logique que « supprimer automatiquement les données inutiles ».

---

## User story 5 - Notifications sobres (sans polling permanent)

- **Contexte** : En tant qu'**utilisateur**, je veux **consulter mes messages**, afin de **ne pas solliciter le réseau en permanence quand la page est ouverte sans action**.
- **Objectif** : supprimer le polling **7 s** ; refresh à l'ouverture ou sur action manuelle.
- **Bonne pratique d'éco-conception ciblée** : *Réduire le nombre de requêtes HTTP* - pas d'appels répétés sans besoin (MentorPromo - processus & exploitation).
- **KPI associé** : appels `/api/notifications` par minute en idle (cible : **0**).
- **Repo ou écran concerné** : `NotificationsPage`, `setInterval(refresh, 7000)`.
- **Critère de réussite** : 0 appel API après chargement initial pendant 5 min sans interaction.
- **Niveau de priorité** : **moyenne**

**Justification méthodologique MentorPromo** : surveiller et réduire les appels API inutiles.

---

## Notes

- US-1 à US-3 = **3 pistes éco-conception de mon ACV** appliquées sur heavy-hub.
- US-4 et US-5 = bonnes pratiques complémentaires du même référentiel.
- Mesures : `docs/audit-initial.md` → implémentation → `docs/audit-final.md`.

