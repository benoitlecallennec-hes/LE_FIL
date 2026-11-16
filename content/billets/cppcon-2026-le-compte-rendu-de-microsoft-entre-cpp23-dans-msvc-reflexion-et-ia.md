---
title: "CppCon 2026 : le compte rendu de Microsoft, entre C++23 dans MSVC, réflexion et IA"
date: 2026-11-14T21:48:28.000Z
source: isocpp.org
link: https://isocpp.org//blog/2026/11/microsoft-cppcon-2026-trip-report-michael-price
tags:
  - cpp
lang: en
level: beginner
format: article
reading_time: 5
image: https://isocpp.org/files/img/mstripreport-cpp26.png
summary: "Michael Price revient sur la participation de Microsoft à CppCon 2026 à Aurora : les sessions de l'équipe C++, l'arrivée de `/std:c++23` dans MSVC et les thèmes qui ont dominé les échanges avec les développeurs."
takeaways:
  - MSVC Build Tools v14.52 (novembre 2026) implémente tout C++23 et ajoute `/std:c++23`.
  - "Retour d'expérience de Rare sur le passage à `/W4 /WX` : l'avertissement C4189 est passé de près de 10 000 occurrences à zéro."
  - Sample-based PGO (SPGO) est présentée comme l'évolution de la PGO classique pour optimiser le code natif.
  - La réflexion suscite un fort enthousiasme, et les workflows assistés par IA deviennent courants chez les développeurs C++.
status: rejected
draft: true
featured: false
---
Microsoft a tenu un stand à CppCon 2026, à Aurora (Colorado). L'équipe y a rencontré des développeurs de jeux, de systèmes financiers, d'outils de développement, de logiciels embarqués et d'outils scientifiques. Certains utilisent les dernières fonctionnalités du langage, d'autres maintiennent et modernisent des systèmes en production depuis des décennies. Les discussions ont surtout porté sur le débogage, les performances, les systèmes de build et le C++ moderne. De plus en plus de visiteurs cherchent aussi comment l'IA peut les aider. Les participants qui répondaient au sondage annuel Microsoft C++ entraient dans un tirage au sort quotidien de sets LEGO.

Le compte rendu complet, publié sur le blog de l'équipe C++ de Microsoft, présente plusieurs sessions. Keith Stockdale (Rare) a raconté le passage de sa base de code à `/W4 /WX`, alors que des milliers d'avertissements y étaient jusque-là masqués par des `#pragma` ou par des options de ligne de commande. L'avertissement le plus fréquent, C4189 (variable locale initialisée mais jamais utilisée), est passé de près de 10 000 occurrences uniques à zéro. Augustin Popa et David Li ont présenté les nouveautés de Visual Studio et de GitHub Copilot pour moderniser le code, réduire les temps de build et déboguer. Ils ont surtout annoncé MSVC Build Tools v14.52 pour novembre, déjà disponible en release candidate : cette version implémentera toutes les fonctionnalités de C++23, ajoutera l'option `/std:c++23` et restera compatible avec C++/CLI. Environ 650 bogues ont été corrigés depuis la conférence précédente.

Côté Visual Studio Code, Sinem Akinci et Ben McMorran ont montré comment passer d'une expérience agentique à une autre selon la tâche et comment personnaliser le comportement de l'IA (BYOK, skills, agents personnalisés, hooks). Eric Brumer a parlé des performances du code natif : fonctionnement des processeurs modernes, utilisation des profileurs et Sample-based PGO (SPGO), une évolution de la PGO classique déjà utilisée en interne chez Microsoft. Ion Todirel a proposé une grille de lecture des différentes notions de « sûreté » en C++. Il a rappelé que le C++ n'est pas le C et qu'il a ajouté des fonctionnalités précisément pour corriger des problèmes hérités de cette compatibilité.

Trois tendances ressortent des échanges : un fort enthousiasme pour la réflexion, que l'auteur juge potentiellement aussi marquante que les templates ; la performance et le contrôle, toujours cités comme les premières raisons de choisir le C++ ; et la banalisation des workflows à base de LLM, jusque dans des environnements restreints ou isolés du réseau. Les enregistrements des sessions de CppCon 2026 sont mis en ligne au fur et à mesure.
