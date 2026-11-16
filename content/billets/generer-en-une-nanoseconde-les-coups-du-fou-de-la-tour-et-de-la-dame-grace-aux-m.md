---
title: Générer en une nanoseconde les coups du fou, de la tour et de la dame grâce aux magic bitboards
date: 2026-11-13T15:00:05.000Z
source: CppCon
link: https://www.youtube.com/watch?v=i4XWhpoKPDs
tags:
  - cpp
lang: en
level: advanced
format: video
image: https://i2.ytimg.com/vi/i4XWhpoKPDs/hqdefault.jpg
summary: À CppCon 2026, Aryan Naraghi montre comment son moteur d'échecs FollyChess génère les coups des pièces glissantes en une nanoseconde environ grâce aux magic bitboards, et comment produire les tables nécessaires au moment de la compilation.
takeaways:
  - Calculer les coups des pièces glissantes à la demande prend plus de 20 ns, et une table naïve exigerait des zettaoctets.
  - Les magic bitboards, une technique de hachage parfait, ramènent ce coût à environ 1 ns.
  - L'instruction matérielle `PEXT` est comparée à une version logicielle portable.
  - Face aux limites de `consteval`, les tables peuvent être générées pendant la compilation avec Bazel.
status: published
featured: false
edition: 2026-W47
---
Un moteur d'échecs doit explorer des millions de positions par seconde, et la génération des coups en est souvent le goulot d'étranglement. Pour le cavalier, le roi et les pions, elle ne coûte qu'environ une nanoseconde. Le fou, la tour et la dame, les « pièces glissantes », sont un cas à part : leurs déplacements dépendent de la position des autres pièces. Les calculer à la demande prend plus de 20 nanosecondes, et une table de correspondance naïve demanderait des zettaoctets de mémoire.

La conférence commence par présenter les structures de données centrales d'un moteur d'échecs et la logique de la génération de coups. Elle aborde ensuite les « magic bitboards », une technique de hachage parfait qui ramène la génération des coups des pièces glissantes à environ une nanoseconde.

Aryan Naraghi montre comment l'implémenter en C++ moderne. Il compare l'instruction matérielle `PEXT` (Parallel Bits Extract) à une approche logicielle portable. Il traite enfin des difficultés pratiques que pose la génération des tables nécessaires aux magic bitboards : les limites de `consteval`, puis l'intégration de cette génération dans la compilation avec Bazel. Les exemples de code viennent de FollyChess, son moteur d'échecs en C++.
