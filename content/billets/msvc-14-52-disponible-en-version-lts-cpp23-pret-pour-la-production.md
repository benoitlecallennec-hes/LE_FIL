---
title: "MSVC 14.52 disponible en version LTS : C++23 prêt pour la production"
date: 2026-11-16T22:17:16.000Z
source: Microsoft C++ Team Blog
link: https://devblogs.microsoft.com/cppblog/msvc-build-tools-version-14-52-now-generally-available/
tags:
  - cpp
feed_tag: Announcement
lang: en
level: intermediate
format: article
reading_time: 4
summary: Microsoft publie en disponibilité générale les MSVC Build Tools 14.52, une version à support long (LTS) qui rend C++23 utilisable en production avec `/std:c++23` et livre une bibliothèque standard complète pour C++23.
takeaways:
  - MSVC 14.52 est une version LTS, livrée avec VS 2026 18.12 (Stable) et 18.11 (LTSC)
  - "`/std:c++23` est prêt pour la production ; `constexpr` pour `<cmath>` et `<cstdlib>` s'active avec `/Zc:cmath`"
  - Le ciblage APX sur x64 et le C++ Dynamic Debugging sont désormais en disponibilité générale
  - Modules C++ plus fiables et algorithmes de la STL vectorisés en SVE sur ARM64
missing:
  - image principale de la page
status: rejected
draft: true
featured: false
---
Les MSVC Build Tools 14.52 sont disponibles en version à support long (LTS). Ils sont livrés avec Visual Studio 2026 18.12 sur le canal Stable et avec Visual Studio 2026 LTSC 18.11, deux versions sorties le 10 novembre. Selon Microsoft, cette version n'était pas prévue comme LTS dans le cycle de vie actuel. Elle l'est devenue pour aider les équipes C++ à mettre à jour leurs outils et à rester alignées sur Visual Studio. Le canal Stable continue de recevoir des nouveautés. LTSC s'adresse aux organisations qui veulent limiter les changements dans leur environnement. Une annonce à venir détaillera le cycle de vie de MSVC.

Côté langage, `/std:c++23` est prêt pour la production et la STL couvre toutes les fonctionnalités de C++23. Le support `constexpr` de `<cmath>` et `<cstdlib>` reste à activer avec `/Zc:cmath`. L'optimiseur progresse sur l'inlining, les boucles, la vectorisation et la sélection d'instructions. Sur ARM64, les opérations vectorielles et l'appariement des chargements s'améliorent. Sur x64, la génération de code APX s'étend : le ciblage des Intel Advanced Performance Extensions est désormais pris en charge en production. Les modules C++ deviennent nettement plus fiables, avec de meilleurs diagnostics, une meilleure gestion du code dépendant et des retours plus clairs à l'édition de liens.

Côté bibliothèques et outils, la version apporte des algorithmes de la STL vectorisés en SVE sur ARM64. AddressSanitizer gagne en performances et décode mieux les instructions, et les gros flux d'informations de débogage sont mieux gérés. Le C++ Dynamic Debugging, qui facilite l'inspection des builds optimisés, passe en disponibilité générale. De nombreux correctifs signalés par la communauté sont aussi intégrés.

Pour migrer, il faut installer la charge de travail C++ et les composants MSVC 14.52 avec le Visual Studio Installer. Avec CMake, on vérifie que la configuration sélectionne bien MSVC 14.52. Les projets MSBuild (`.vcxproj`) issus de Visual Studio 2022 ou d'une version antérieure se reciblent avec l'assistant de configuration (`Project > Retarget solution`) ou en changeant le Platform Toolset. L'agent `@Modernize` de GitHub Copilot peut aider à corriger les problèmes de mise à jour. Les préversions restent accessibles via Visual Studio 2026 Insiders, mais elles ne reçoivent pas de mises à jour de maintenance.
