---
title: "Blender 5.3 introduit les projets : variables de chemins, bibliothèques d'assets et gestion des couleurs propres à chaque production"
date: 2026-11-13T14:07:13.000Z
source: Blender Developers Blog
link: https://code.blender.org/2026/11/project-setup-whats-in-and-whats-next/
tags:
  - 3d
feed_tag: General Development
lang: en
level: intermediate
format: article
reading_time: 4
image: https://code.blender.org/wp-content/uploads/2026/11/projects_menu_file-1135x1280-1-908x1024.webp?x95207
summary: Avec Project Setup, Blender 5.3 permet de regrouper des fichiers dans un projet doté de ses propres réglages, variables et bibliothèques d'assets. L'équipe de développement détaille ce qui est déjà livré et ce qui est prévu pour la version 5.4.
takeaways:
  - Un projet Blender est un dossier contenant `.blender_project/project.toml`, détecté automatiquement en remontant l'arborescence depuis le `.blend` ouvert.
  - Les variables `{project_root}`, `{project_name}` et les variables personnalisées rendent les chemins de sortie portables d'une machine à l'autre.
  - Bibliothèques d'assets, configuration OpenColorIO et écran d'accueil peuvent être définis par projet, et ces réglages priment sur les préférences utilisateur.
  - Blender 5.4 vise des add-ons livrés par projet et des variables acceptées sur presque tous les chemins.
status: published
featured: false
edition: 2026-W47
---
Jusqu'ici, les préférences de Blender étaient communes à tous les projets, et c'était aux utilisateurs et aux équipes de garder leurs fichiers et ressources en ordre. Blender 5.3 introduit Project Setup, un système optionnel qui définit des réglages, des bibliothèques d'assets et des variables partagées pour chaque projet. Un projet est un dossier racine reconnu à la présence d'un dossier `.blender_project`, qui contient un fichier de configuration `project.toml` géré par Blender. À l'ouverture d'un `.blend`, Blender remonte l'arborescence pour trouver ce marqueur et active le projet s'il le trouve. On peut aussi ouvrir un projet en passant un chemin à Blender au lancement, ou depuis le nouveau menu File > Project. Les réglages du projet ont toujours la priorité sur les préférences de l'utilisateur. Ce sont de simples fichiers texte, que des scripts externes peuvent produire et que l'on peut comparer (diff) ou versionner avec le reste de la production.

La version 5.3 apporte d'abord des variables de projet et des modèles de chemins (path templates). Les chemins de sortie peuvent utiliser `{project_root}` et `{project_name}`, et restent donc valides si le dossier change de disque, de machine ou d'artiste, sans recourir partout aux chemins relatifs au `.blend`. On peut aussi définir ses propres variables, comme un nom de personnage, un code de séquence ou un dossier fréquent : il suffit de modifier la valeur une fois pour que tous les chemins qui l'utilisent suivent. Chaque projet peut aussi déclarer ses bibliothèques d'assets, visibles dans l'Asset Browser, les Asset Shelves et les menus Add. Il fixe également sa configuration OpenColorIO (celle par défaut de Blender, ACES 1.3, ACES 2.0 ou une configuration personnalisée), si bien que toute l'équipe partage le même pipeline couleur. Enfin, un projet peut avoir son propre écran d'accueil, qui liste aussi les fichiers récents du projet.

Le travail continue dans Blender 5.4. La priorité est de permettre à un projet de fournir son propre dossier d'extensions (add-ons), avec des réglages d'add-on propres à chaque projet. L'équipe veut aussi accepter les variables sur (presque) tous les chemins de Blender et afficher les variables intégrées comme `{project_root}` dans la liste des variables du projet. Pour ceux qui passent d'un projet à l'autre, plusieurs améliorations sont prévues : un sélecteur de projet dans le File Browser, un Save As limité au projet courant et des variables laissées non résolues lors d'un Save As Copy. L'avancement est suivi dans la tâche « Project Setup 5.4 targets ».
