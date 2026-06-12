# ACV Flash

---

## 1. Mon service numérique


| Élément      | Description                                                            |
| ------------ | ---------------------------------------------------------------------- |
| **Service**  | MentorPromo                                                            |
| **Type**     | Plateforme web                                                         |
| **Fonction** | Simple pour connecter les nouveaux étudiants avec les anciens diplômés |


### Utilisateurs et usage


| Indicateur                | Valeur                             |
| ------------------------- | ---------------------------------- |
| **Utilisateurs**          | Étudiant·es / Prof / Professionnel |
| **Fréquence**             | Une fois / semaine                 |
| **Durée**                 | 30 min / session                   |
| **Nombre d'utilisateurs** | 100                                |


### Problème adressé

Ce service permet d'avoir des réponses précises sur des questions spécifiques, réssoudre le problème de l'entraide mais dans le désordre :

- WhatsApp, Discord, groupes de promo : les informations utiles se perdent dans le flux de messages.
- Des anciens sollicités au hasard.
- Les anciens diplômés se retrouvent spammés sans filtre, rendant l'entraide inefficace et épuisante.

### Stack technique

Il repose sur une plateforme web, hébergée dans le cloud, avec :

- un **front-end** développé en **Next.js**
- un **back-end** en **Node.js**
- des bases de données **PostgreSQL**

---

## 2. Unité fonctionnelle (UF)

Chaque session de mentorat dure au minimum **30 minutes**.

> Contexte d'usage : réalisé depuis (reste à voir…).

---

## 3. Le cycle de vie d'un service numérique

### 3.1 Material Extraction  Fabrication des équipements (serveurs, terminaux…)

- Fabrication du MacBook utilisé pour coder le back-end
- Extraction de matières pour la batterie du laptop utilisateur
- Production des câbles réseau / fibre / routeurs
- Fabrication des machines qui hébergent l'API et la base PostgreSQL

### 3.2 Manufacturing  Déploiement technique (dev, stockage, infra…)

- Compilation du code Node.js du backend
- Fabrication des écrans utilisés pour le design UI
- Les serveurs pour héberger la base de données
- Compilation Next.js et mise en ligne (hébergement web/CDN pour servir pages et assets)
- Déploiement du backend + API : exécution du service Node.js, configuration HTTPS

### 3.3 Packaging & Transport  Distribution / réseau (FAI, transport de données)

- Acheminement des flux de données via les réseaux fibre/4G/5G
- Téléchargement des dépendances npm pendant le dev
- Connexion via wifi ou ethernet

### 3.4 Use Phase  Phase d'usage (temps passé, fonctionnalités, fréquence)

- Usage du navigateur pour accéder à l'interface
- Calcul de l'analyse via appel API depuis le back-end
- Écriture / lecture en base de données (score, rapport)
- Consultation annuaire + filtres : chargements répétés
- Ouverture profils mentors : lecture des infos + tags + disponibilités
- Envoi de demande de contact : saisie du message, validations, envoi, réception email/notification (usage plus « lourd » ponctuellement)

### 3.5 End of Life  Fin de vie (renouvellement, obsolescence, effacement)

- Obsolescence du laptop utilisé pour coder (changement tous les 3-4 ans)
- Archivage / effacement des logs d'usage
- Suppression de comptes utilisateurs inactifs
- Suppression/anonymisation des données : effacer demandes de contact et comptes (RGPD), politiques de rétention
- Archivage & purge : nettoyer anciennes demandes, logs, sauvegardes
- Fin de service / migration : exporter les données utiles, couper l'infra, décommissionner bases/serveurs, résilier domaine/outil email

---

## 4. Que puis-je améliorer sur mon service ?

### Modèles économiques circulaires

- Mutualiser l'hébergement avec une offre cloud partagée plutôt qu'une infrastructure surdimensionnée
- Allonger la durée de vie du service avec une architecture simple et maintenable, pour éviter les refontes fréquentes

### Design & développement du produit

- Supprimer les librairies inutilisées ou réduire la taille du bundle JS
- Remplacer des polices externes par des polices système
- Réduire le nombre de requêtes HTTP par page
- Optimiser les images, icônes et scripts pour diminuer les requêtes réseau et la consommation de bande passante
- Éviter les fonctionnalités coûteuses comme vidéo intégrée, animations lourdes ou chargements inutiles
- Alléger les pages de l'annuaire, du profil mentor et du formulaire pour réduire le poids chargé côté navigateur

### Changement de comportements utilisateurs

- Encourager des messages clairs et ciblés pour éviter les envois multiples ou inutiles
- Favoriser un usage sobre : accès rapide à l'information, parcours court, peu de clics

### Processus & exploitation

- Passer à un hébergeur français
- Automatiser l'extinction des serveurs hors usage
- Mettre en place une purge régulière des anciennes demandes de contact, logs et données inutiles
- Surveiller les performances pour repérer les pages trop lourdes ou les appels API inutiles

---

## 5. Mes 3 pistes d'éco-conception à explorer

1. **Réduire le poids des pages** en simplifiant le design, compressant les images et limiter le JavaScript
2. **Supprimer automatiquement les données inutiles** comme les anciennes demandes de contact et les logs trop anciens
3. **Rendre le parcours utilisateur plus court** : recherche plus précise, meilleurs filtres, moins de pages consultées inutilement

---

## 6. Repo starter heavy-hub (terrain d'expérimentation)


| Catégorie formation                                   | Repo associé  |
| ----------------------------------------------------- | ------------- |
| Plateforme de contenu / espace membre / LMS / portail | **heavy-hub** |


**Pourquoi heavy-hub ?** MentorPromo appartient à cette catégorie. **Appliquer la même méthodologie d'éco-conception** sur heavy-hub pour mesurer et prioriser des actions concrètes.

