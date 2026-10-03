# Cartographie de cadrage - MentorPromo × heavy-hub

**Auteure** : Khady Diop · **Mise à jour** : 03/10/2026
**Sources** : [ACV Flash](acv-flash-khady.md) ([Miro](https://miro.com/welcomeonboard/azBKaFlpR1RqR1hRc2FBT0lnbUlKb3hIdUNuWTVmbmgvckpIdGd0SlU0VFEwNCthNXRLa09JWjNuTERxNEJXNHBzajYvVmFMaE9HOVJQdnBDZWlnd1JON1UyOGU2STZxYWdmcy9NTVJWQ0gyREt4bEFyRXRsYlR1TTB6QXNMMFd3VHhHVHd5UWtSM1BidUtUYmxycDRnPT0hdjE=?share_link_id=643304053230)) · [Bonnes pratiques](https://docs.google.com/spreadsheets/d/1WRLaJbdfu5FhS5ES2Kp1FJ6S3ScEpGz2/edit?usp=sharing) · [Audit initial](audit-initial.md) · [Analyse EcoIndex (Excel)](https://docs.google.com/spreadsheets/d/1u0nX10ybVjA_yS0mieFsL0OivxkbP2KK/edit?usp=sharing) · [Backlog](../backlog.md) · [Plan d'action 6 mois (slides)](https://docs.google.com/presentation/d/1kQ9gD2V6eLYpNfeGysgCXbchujL-qUOb/edit?usp=sharing)

---

## 1. Le service réel : MentorPromo

| Élément | Description |
| ------- | ----------- |
| **Fonction** | Mettre en relation les nouveaux·elles étudiant·es avec les ancien·nes diplômé·es pour obtenir des réponses précises |
| **Problème résolu** | Entraide dispersée (WhatsApp, Discord), anciens sollicités au hasard et sans filtre |
| **Utilisateurs** | Étudiant·es, professeur·es, professionnel·les : environ **100 utilisateurs** |
| **Usage** | **1 session / semaine**, **30 min** par session |
| **Stack** | Next.js (front), Node.js (API), PostgreSQL, hébergement cloud |
| **Unité fonctionnelle** | **Une session de mentorat de 30 min** : connexion, recherche d'un mentor dans l'annuaire, consultation de son profil, envoi ou suivi d'une demande de contact |

**Mécanismes d'impact identifiés dans l'ACV** (phase d'usage, la seule sur laquelle le code agit) :

- chargements répétés de l'annuaire et des filtres ;
- ouverture des profils (infos, tags, disponibilités) ;
- appels API et lectures/écritures en base ;
- stockage de données qui ne servent plus (anciennes demandes, logs).

---

## 2. Rattachement à une catégorie et à un repo miroir

| Catégorie | Repo | Retenu ? | Justification |
| --------- | ---- | -------- | ------------- |
| Site vitrine / média / institutionnel | heavy-showcase | Non | MentorPromo n'est pas un site de présentation |
| E-commerce / catalogue / réservation | heavy-shop | Non | Pas de transaction ni de panier |
| Dashboard / outil métier / SaaS interne | heavy-ops | Non | Pas un outil interne de pilotage |
| **Plateforme de contenu / espace membre / portail** | **heavy-hub** | **Oui** | Espace connecté, annuaire filtrable, fiches, messages, profil |

### Correspondance des écrans

| Écran MentorPromo | Écran heavy-hub | Mécanisme d'impact commun |
| ----------------- | --------------- | ------------------------- |
| Accueil connecté | `/` | Préchargement de toutes les données au démarrage |
| Annuaire des mentors + filtres | `/library` | Liste longue, vignettes, filtres |
| Profil d'un mentor | `/content/:id` | Fiche détaillée + contenus liés |
| Demandes de contact / messages | `/notifications` | Rafraîchissement automatique (polling) |
| Suivi de mes échanges | `/dashboard` | Données agrégées |
| Mon profil | `/profile` | Avatar + recommandations |

---

## 3. Périmètre

| Inclus | Exclu |
| ------ | ----- |
| Phase d'**usage** de l'UF (navigateur ↔ API ↔ données) | Fabrication des terminaux et serveurs (hors de portée du code) |
| Les 6 écrans de heavy-hub listés ci-dessus | Choix et contrat de l'hébergeur réel de MentorPromo (recommandation seulement, M6) |
| Front (React/Vite) et back (Express) de heavy-hub | Base PostgreSQL réelle (simulée par des fichiers JSON dans heavy-hub) |
| Mesures EcoIndex, réseau et stockage, avant / après | Refonte graphique complète |

---

## 4. Contraintes

| Type | Contrainte | Conséquence sur le cadrage |
| ---- | ---------- | -------------------------- |
| Accès au code | Pas la main sur le code réel de MentorPromo | Expérimentation sur heavy-hub, puis transfert documenté |
| Fonctionnelle | Ne pas casser la logique produit (mêmes écrans, mêmes données) | Actions de type « charger moins / plus tard », pas de suppression de fonctionnalité |
| Temps | 6 mois, en parallèle de la formation | Quick wins d'abord (M2-M3), chantiers structurants ensuite (M4-M6) |
| Mesure | Mesures comparables avant / après | Même outil et même protocole avant et après |
| Réglementaire | RGPD : données personnelles des étudiant·es et anciens | Purge et rétention (US-8) |
| Technique | Stack heavy-hub ≠ stack MentorPromo (Vite/Express vs Next.js/PostgreSQL) | On transfère les bonnes pratiques, pas le code |

---

## 5. Objectifs

| Axe | Objectif | Mesure |
| --- | -------- | ------ |
| **Environnemental** | Réduire les requêtes et le poids transféré par session | −20 % de requêtes HTTP par écran, −60 % d'appels API sur `/` |
| **Environnemental** | Ne plus stocker ni transférer de données inutiles | 0 Ko de stockage local, 0 appel en inactivité, rétention de 30 jours |
| **Utilisateur** | Trouver un mentor ou un contenu rapidement | ≤ 3 clics, moins de requêtes sur le parcours |
| **EcoIndex** | Passer chaque écran au-dessus de 90 (A) | Baseline : 87,2 à 88,7 |
| **Transfert** | Une fiche de recommandations directement applicable à MentorPromo | Livrée en M6 |

---

## 6. Indicateurs retenus

| Indicateur | Pourquoi celui-ci | Outil | Avant | Cible |
| ---------- | ----------------- | ----- | --------------- | ----- |
| **Appels API au 1er chargement de `/`** | Mécanisme principal de l'ACV (appels API inutiles) | DevTools Network | 5 | ≤ 2 |
| **Requêtes HTTP par écran** | Composante de l'EcoIndex, liée au réseau et au serveur | EcoIndex | 11 en moyenne | −20 % |
| **Poids transféré par écran (Ko)** | Énergie réseau et terminal | EcoIndex | 237 Ko en moyenne | −15 % |
| **Éléments DOM** | Pèse ×3 dans l'EcoIndex, coût CPU du terminal | EcoIndex | 115 à 186 | −30 % sur `/library` |
| **Appels en inactivité (5 min)** | Notifications « bavardes » | DevTools Network | ~40 | 0 |
| **Stockage local (Ko)** | Données dupliquées sur le terminal | DevTools Application | 5 payloads | 0 Ko |
| **EcoIndex / eau / GES** | Synthèse lisible pour décider et comparer | EcoIndex | 88,0 (A) en moyenne | ≥ 90 |


---

## 7. Priorisation des actions (impact / effort)

| | **Effort faible** | **Effort moyen** |
| --- | --- | --- |
| **Impact fort** | US-1 appels API à la demande · US-5 polling · US-2 images | US-6 pagination (DOM) · US-8 purge · US-7 cache HTTP |
| **Impact moyen** | US-4 stockage local · US-3 parcours | US-9 découpage JS |

Règle retenue : **quick wins à fort impact d'abord** (M2-M3, réalisés), puis les **chantiers structurants** qui font vraiment monter l'EcoIndex (DOM, cache, données) en M4-M6.

---

## 8. Risques et parades

| Risque | Parade |
| ------ | ------ |
| Gains EcoIndex faibles malgré de bons gains réseau | Cibler le DOM et le JS (US-6, US-9), qui pèsent le plus dans le score |
| Régression fonctionnelle (écran vide, données manquantes) | Test de navigation des 6 écrans après chaque US |
| Mesures non comparables | Même protocole avant et après, documenté dans l'Excel |
| Transfert vers MentorPromo non appliqué | Fiche de transfert par US (section « Transfert MentorPromo » du backlog) |
