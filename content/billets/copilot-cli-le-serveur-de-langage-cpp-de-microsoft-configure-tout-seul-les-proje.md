---
title: "Copilot CLI : le serveur de langage C++ de Microsoft configure tout seul les projets MSBuild"
date: 2026-11-10T17:02:48.000Z
source: Microsoft C++ Team Blog
link: https://devblogs.microsoft.com/cppblog/automatic-msbuild-configuration-for-the-microsoft-c-language-server-in-copilot-cli/
tags:
  - cpp
feed_tag: C++
lang: en
level: intermediate
format: article
reading_time: 3
image: https://devblogs.microsoft.com/cppblog/wp-content/uploads/sites/9/2026/11/msbuild-extractor-2.webp
summary: Dans GitHub Copilot CLI, le Microsoft C++ Language Server génère désormais lui-même le fichier compile_commands.json des projets MSBuild au démarrage. Plus besoin de le fournir à la main ni de lancer un skill de configuration.
takeaways:
  - Pour les projets MSBuild, le serveur de langage C++ de Copilot CLI génère compile_commands.json automatiquement au démarrage.
  - Il lit les .vcxproj et les feuilles de propriétés avec les mêmes outils MSBuild que Visual Studio.
  - La génération tourne en arrière-plan, se suit avec /lsp logs et peut être désactivée ou réglée (timeout).
  - CMake et les builds sur mesure passent toujours par un skill ou un compile_commands.json fourni à la main.
status: rejected
draft: true
featured: false
---
Pour bien comprendre du code C++, un serveur de langage doit savoir comment le projet est compilé : options du compilateur, chemins d'inclusion, macros du préprocesseur. C'est le rôle d'une base de compilation comme compile_commands.json. Grâce à elle, des fonctions comme « aller à la définition » ou « trouver les références » retrouvent les bons symboles. Microsoft annonce que son C++ Language Server, utilisé dans GitHub Copilot CLI, sait maintenant produire cette base tout seul pour les projets MSBuild. Il s'appuie pour cela sur les outils C++ et MSBuild de Visual Studio.

Jusqu'ici, il fallait soit fournir soi-même le fichier compile_commands.json, soit laisser Copilot trouver puis lancer un « skill » (un module d'action de l'agent) chargé de le générer. Cette étape prenait du temps et consommait des tokens avant que l'agent puisse exploiter l'analyse sémantique du code. Désormais, le serveur embarque un extracteur de commandes de compilation MSBuild. Il le lance automatiquement au démarrage quand la base doit être générée. Selon Microsoft, la configuration devient plus prévisible et demande moins d'étapes manuelles. On peut par exemple demander tout de suite à Copilot de trouver la définition d'une fonction et la liste de ses appelants.

Concrètement, le serveur lit les chemins d'inclusion, les définitions du préprocesseur et les options de compilation déclarés dans les fichiers .vcxproj et les feuilles de propriétés. Il en tire une base de compilation qui décrit comment chaque fichier source est compilé. La génération tourne en arrière-plan et sa durée dépend de la taille et de la complexité de la solution. La commande /lsp logs permet d'en suivre la progression. On peut régler le délai d'attente (timeout) ou désactiver la fonction dans la configuration ; les changements s'appliquent au démarrage suivant du serveur. L'installation suppose d'accepter la licence (EULA).

Pour les autres systèmes de build, il faut toujours fournir son propre compile_commands.json ou passer par un skill. Pour CMake, on demande à Copilot CLI de « load project », ce qui lance le skill generate-compile-commands d'une version précédente. Pour les builds sur mesure, un guide explique comment écrire son propre skill d'extraction.
