---
title: "« No More Helloworlds » de Nikolai Kutiavin : un livre pour passer du hello world à une vraie application C++"
date: 2026-11-19T23:00:00.000Z
source: sandordargo.com
link: https://www.sandordargo.com/blog/2026/11/20/build-a-real-cpp-app-by-nikolai-kutiavin
tags:
  - cpp
feed_tag: books
lang: en
level: beginner
format: article
reading_time: 4
summary: Sandor Dargo recommande aux débutants en C++, et en développement en général, le livre de Nikolai Kutiavin. Le livre suit toutes les étapes qui mènent à une application C++ livrée, de l'écriture du code jusqu'au pipeline CI/CD.
takeaways:
  - Développer en C++, c'est aussi écrire de l'interface graphique, des tests, des scripts de compilation et un pipeline CI/CD.
  - On crée une interface soit pour servir les utilisateurs d'une bibliothèque, soit pour abstraire une dépendance externe, et chacun de ces deux cas sert à écrire des mocks.
  - Les tests valident le comportement, l'analyse statique valide la forme.
  - Le livre vise les débutants et donne un parcours d'apprentissage choisi plutôt qu'une référence complète du langage.
missing:
  - image principale de la page
status: published
featured: false
edition: 2026-W48
---
Sandor Dargo part d'un souvenir de conférence. Lors d'une lightning talk, un intervenant demandait combien de langages un développeur C++ doit parler. Sa réponse : développer en C++ ne se limite pas à écrire du C++. Il faut au moins un peu de Bash et un peu de Python, et sans doute davantage. Le livre de Nikolai Kutiavin part du même constat. Il ne se limite pas au langage et parcourt toutes les étapes qui mènent à un logiciel livré : écrire le code de production, construire une interface graphique en C++ pour l'application, la tester (tests unitaires, d'intégration et manuels), compiler le code, puis mettre en place un pipeline CI/CD avec Git.

Le livre s'adresse d'abord à ceux qui débutent en développement logiciel et ont choisi le C++. Les développeurs expérimentés ne sont pas le public visé, mais Sandor Dargo y a quand même trouvé quelques idées utiles. La première : on crée en général une interface pour l'une de deux raisons. Soit pour offrir une abstraction aux utilisateurs d'une bibliothèque, soit pour abstraire un sous-système externe dont la bibliothèque se sert. Sandor Dargo relie ces deux cas aux mocks. Dans le premier, ce sont vos utilisateurs qui simulent votre bibliothèque. Dans le second, c'est vous qui simulez une dépendance, comme une base de données.

L'auteur insiste aussi sur la validation du code dès la compilation, en plus des tests. Sandor Dargo le résume ainsi : les tests valident le comportement, l'analyse statique valide la forme. Le livre rappelle enfin que développer, c'est aussi intégrer des changements sans casser l'existant, et que Git et l'intégration continue servent à cela.

Sandor Dargo prévient que le livre ne suffira pas pour apprendre le langage lui-même : on ne peut pas couvrir autant de sujets en détail. Certains thèmes peuvent aussi paraître hors sujet dans un livre sur le C++. Pour lui, l'intérêt du livre est ailleurs : c'est une liste choisie par un développeur expérimenté de ce qu'il faut apprendre, avec assez de matière pour savoir où creuser. Il juge cette sélection précieuse à l'heure où les outils d'IA peuvent eux aussi expliquer ces sujets.
