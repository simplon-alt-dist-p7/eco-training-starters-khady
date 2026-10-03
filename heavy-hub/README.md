# heavy-hub

## Contexte pedagogique

Repo miroir pedagogique d une plateforme de contenu / espace membre / portail. Il sert a etudier la sobriete de navigation, la limitation du prechargement et la reduction des appels API inutiles dans un espace connecte.

Ce projet est volontairement fonctionnel mais non optimise. Il sert de support d analyse et d experimentation dans un cadre de formation.

## Livrables - Brief « Cadrer un service numérique pour une mise en œuvre responsable »

**Service réel** : MentorPromo (mise en relation étudiant·es / ancien·nes diplômé·es) · **Catégorie** : plateforme de contenu / espace membre / portail · **Repo miroir** : heavy-hub

| # | Livrable demandé | Fichier / lien |
| - | ---------------- | -------------- |
| 1 | Cartographie de cadrage du projet | [docs/cartographie-cadrage.md](docs/cartographie-cadrage.md) |
| 2 | Slides du plan d'action sur 6 mois + contenus des slides précédentes | [Slides plan d'action 6 mois](https://docs.google.com/presentation/d/1kQ9gD2V6eLYpNfeGysgCXbchujL-qUOb/edit?usp=sharing) · ACV (slides précédentes) : [Miro](https://miro.com/welcomeonboard/azBKaFlpR1RqR1hRc2FBT0lnbUlKb3hIdUNuWTVmbmgvckpIdGd0SlU0VFEwNCthNXRLa09JWjNuTERxNEJXNHBzajYvVmFMaE9HOVJQdnBDZWlnd1JON1UyOGU2STZxYWdmcy9NTVJWQ0gyREt4bEFyRXRsYlR1TTB6QXNMMFd3VHhHVHd5UWtSM1BidUtUYmxycDRnPT0hdjE=?share_link_id=643304053230) |
| 3 | Fichier Excel d'analyse EcoIndex | [Analyse EcoIndex (Google Sheets)](https://docs.google.com/spreadsheets/d/1u0nX10ybVjA_yS0mieFsL0OivxkbP2KK/edit?usp=sharing) |
| 4 | backlog.md (≥ 3 user stories) | [backlog.md](backlog.md) - 9 US (5 réalisées, 4 planifiées) |
| 5 | Lien vers le dépôt Git | https://github.com/simplon-alt-dist-p7/eco-training-starters-khady |

**Documents d'appui**

| Document | Rôle |
| -------- | ---- |
| [docs/acv-flash-khady.md](docs/acv-flash-khady.md) | ACV Flash de MentorPromo (Brief #1) : UF, cycle de vie, 3 pistes |
| [Bonnes pratiques (Google Sheets)](https://docs.google.com/spreadsheets/d/1WRLaJbdfu5FhS5ES2Kp1FJ6S3ScEpGz2/edit?usp=sharing) | Référentiel de bonnes pratiques d'éco-conception |
| [docs/audit-initial.md](docs/audit-initial.md) | Baseline avant implémentation |
| [docs/audit-final.md](docs/audit-final.md) | Résultats après US-1 à US-5 |

**Fil conducteur** : ACV Flash → cartographie de cadrage → baseline (audit initial + EcoIndex) → backlog priorisé → implémentation sur heavy-hub → audit final → plan sur 6 mois et transfert vers MentorPromo.

## Perimetre fonctionnel

- Home connectee
- Bibliotheque de contenus
- Fiche contenu
- Dashboard utilisateur
- Notifications
- Profil

## Anti-patterns presents

- medias lourds
- contenus precharges inutilement
- avatars non optimises
- notifications bavardes
- prefetch excessif
- duplication de composants
- appels inutiles au chargement
- stockage local superflu

## Lancement

`npm install`

`npm run dev`

Frontend: http://localhost:5173

Backend: http://localhost:4100

## Mesure et outillage

- Lighthouse sur home et bibliotheque
- EcoIndex sur home connectee
- poids medias et snapshots locaux
- nombre d appels API au premier chargement

### Commandes utiles

- `npm run analyze`
- `npm run lighthouse`
- Lighthouse dans le navigateur Chrome
- EcoIndex via l'extension ou le site dedie
