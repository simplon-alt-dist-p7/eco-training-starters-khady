# Audit final - heavy-hub

**Date** : 03/10/2026 - statut : après implémentation des US-1 à US-5 (M2-M3 de la roadmap).
**Données détaillées** : [Analyse EcoIndex (Excel)](https://docs.google.com/spreadsheets/d/1u0nX10ybVjA_yS0mieFsL0OivxkbP2KK/edit?usp=sharing).

---

## 1. EcoIndex - avant / après


| Page                 | EcoIndex AVANT | EcoIndex APRÈS | Écart    | Requêtes avant → après | Poids avant → après (Ko) | Eau avant → après (cl) | GES avant → après (gCO2e) |
| -------------------- | -------------- | -------------- | -------- | ---------------------- | ------------------------ | ---------------------- | ------------------------- |
| `/`                  | 88,35 (A)      | **89,27 (A)**  | +0,92    | 12 → 9                 | 240 → 203                | 1,85 → 1,82            | 1,23 → 1,21               |
| `/library`           | 87,74 (A)      | **88,12 (A)**  | +0,38    | 11 → 9                 | 240 → 213                | 1,87 → 1,86            | 1,25 → 1,24               |
| `/content/content-1` | 87,52 (A)      | **87,91 (A)**  | +0,39    | 12 → 10                | 245 → 218                | 1,87 → 1,86            | 1,25 → 1,24               |
| `/dashboard`         | 88,72 (A)      | **89,15 (A)**  | +0,43    | 11 → 9                 | 240 → 204                | 1,84 → 1,83            | 1,23 → 1,22               |
| `/notifications`     | 87,18 (A)      | **87,45 (A)**  | +0,27    | 8 → 6                  | 209 → 200                | 1,88 → 1,88            | 1,26 → 1,25               |
| `/profile`           | 88,69 (A)      | **88,98 (A)**  | +0,29    | 12 → 11                | 250 → 216                | 1,84 → 1,83            | 1,23 → 1,22               |
| **Moyenne**          | **88,03**      | **88,48**      | **+0,45** | **11 → 9**            | **237 → 209**            | **1,86 → 1,85**        | **1,24 → 1,23**           |

**Cible révisée** : l'objectif initial « ≥ +10 pts » n'était pas réaliste pour ces US. L'EcoIndex dépend d'abord du **DOM** (poids ×3) et du **JavaScript**, que les US 1 à 5 ne modifient pas. Nouvelle cible : **≥ 90 (A) sur chaque écran à la fin de M6**, via US-6 (DOM) et US-9 (JS).

---

## 2. Résultats par user story

### US-1 - APIs à la demande


| Métrique                             | Avant    | Cible après            | Mesuré après |
| ------------------------------------ | -------- | ---------------------- | ------------ |
| Requêtes `/api/*` au chargement `/`  | 5        | ≤ 2                    | **2** (home + dashboard) |
| Endpoints chargés sur `/` uniquement | 5 (tous) | home + dashboard       | **2** - library, profile et notifications sont chargés à la navigation, une seule fois chacun |


---

### US-2 - Allègement des images


| Métrique                              | Avant      | Cible après         | Mesuré après |
| -------------------------------------- | ---------- | ------------------ | ------------ |
| Images transférées à l'ouverture de `/library` | 30,9 Ko | −30 % minimum | **6,7 Ko (−78 %)** |
| Poids assets repo (`npm run analyze`) | 40,5 Ko    | −30 % minimum       | **8,3 Ko (−79 %)** |
| Lazy loading sur `ContentCard`        | Non        | Oui                 | **Oui** (`loading="lazy"`, `decoding="async"`) |

Les 9 cartes réutilisent 3 fichiers SVG : le gain vient surtout de la compression. Le lazy loading servira avec des vignettes distinctes.

---

### US-3 - Parcours plus court


| Métrique                                  | Avant      | Cible après         | Mesuré après |
| ------------------------------------------ | ---------- | ------------------- | ------------ |
| Clics Accueil → Bibliothèque → Fiche      | 2 | ≤ 3 | **2** - déjà conforme, filtres sans rechargement |
| Requêtes API sur le parcours | 6 (5 au démarrage + 1 `/api/content/:id`) | réduction | **4** (2 au démarrage + 1 `/api/library` + 1 `/api/content/:id`) |


---

### US-4 - Stockage local


| Métrique              | Avant         | Cible après                | Mesuré après |
| --------------------- | ------------- | -------------------------- | ------------ |
| Taille `hub-snapshot` | Présent (5 payloads sérialisés) | 0 Ko | **0 Ko** - localStorage vide après visite des 6 écrans |


---

### US-5 - Notifications sobres


| Métrique                                       | Avant       | Cible après                     | Mesuré après |
| ---------------------------------------------- | ----------- | ------------------------------- | ------------ |
| Appels `/api/notifications` en 5 min d'inactivité | ~40      | 0                               | **0** - `setInterval` supprimé |
| Rafraîchissement                               | Polling 7 s | À l'ouverture + action manuelle | **À l'ouverture + bouton « Actualiser »** (1 appel par clic) |


---

## 3. Bilan qualitatif

### Gains environnementaux observés

- **Réseau** : −2 requêtes par écran en moyenne, −12 % de poids transféré, et surtout **plus aucun appel en arrière-plan**. Une page Messages ouverte 30 min (durée de l'UF) évite environ 250 appels.
- **Terminal** : plus de copie des données dans le localStorage, des images 4,6 fois plus légères et moins de traitement JavaScript au démarrage.
- **Serveur** : 3 endpoints de moins sollicités à chaque connexion, et plus de polling. À l'échelle de MentorPromo (100 utilisateurs, 1 session de 30 min par semaine), le polling pouvait représenter jusqu'à environ 25 000 appels par semaine si la page Messages restait ouverte pendant la session.

### Ce que l'EcoIndex ne montre pas

L'EcoIndex mesure une page au chargement. Il ne voit pas les appels répétés en inactivité, qui sont le gain le plus important ici. Il faut donc suivre les **KPI réseau** en plus du score.

### Limites / actions restantes (roadmap M4-M6)


| Action                             | US   | Horizon | Lien ACV                                 |
| ---------------------------------- | ---- | ------- | ---------------------------------------- |
| Pagination et filtres côté serveur | US-6 | M4      | Piste 1 + piste 3                        |
| Cache HTTP (`no-store` → `max-age` / `ETag`) | US-7 | M5 | Processus & exploitation         |
| Purge automatique notifications et logs (30 j) | US-8 | M5 | Piste 2 - supprimer les données inutiles |
| Découpage du JS par route          | US-9 | M6      | Piste 1 - réduire le JavaScript          |
| Suivi EcoIndex continu + hébergeur français | - | M6 | Surveiller pages lourdes et API inutiles |
