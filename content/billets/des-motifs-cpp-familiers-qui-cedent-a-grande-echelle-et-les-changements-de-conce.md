---
title: Des motifs C++ familiers qui cèdent à grande échelle, et les changements de conception qui les évitent
date: 2026-11-23T15:00:23.000Z
source: CppCon
link: https://www.youtube.com/watch?v=NWAZlRj0Xzk
tags:
  - cpp
lang: en
level: intermediate
format: video
image: https://i3.ytimg.com/vi/NWAZlRj0Xzk/hqdefault.jpg
summary: "À CppCon 2026, Divya Chandrasekar et Devpriya Dave, de Bloomberg, décrivent trois défauts de conception courants dans les grands systèmes C++. Elles proposent pour chacun un changement de conception : des durées de vie explicites, des dépendances injectées et des interfaces contraintes par `std::span`, `std::optional`, `std::variant` et des types forts."
takeaways:
  - Préférer des limites de durée de vie explicites à la possession partagée, qui prolonge les durées de vie sans qu'on le voie.
  - Remplacer les singletons et le couplage global implicite par des dépendances injectées.
  - Contraindre les interfaces avec `std::span`, `std::optional`, `std::variant` et des types forts plutôt que d'exposer des API permissives.
missing:
  - contenu de la vidéo
  - diapositives (lien vers le dossier général seulement)
  - durée de la vidéo
  - date de publication
status: selected
draft: true
featured: false
edition: 2026-W48
---
Pour les deux intervenantes, certains des bugs C++ les plus coûteux ne viennent pas de fonctionnalités obscures du langage. Ils naissent dans du code qui paraît raisonnable : une possession partagée qui prolonge discrètement les durées de vie, des singletons devenus des dépendances invisibles, ou des choix faits pour la performance qui finissent figés dans l'architecture.

La présentation examine ces défauts de conception dans des systèmes C++ de grande taille et propose trois changements. Pour les durées de vie, il s'agit de passer de la possession partagée à des limites de durée de vie explicites. Pour le couplage, le couplage global implicite cède la place à des dépendances injectées. Pour les interfaces, les API trop permissives sont remplacées par des interfaces contraintes, construites avec `std::span`, `std::optional`, `std::variant` et des types forts.

Chaque changement suit le même plan : le défaut qu'il corrige, la solution, puis la règle de conception qui en découle. L'objectif est de fournir des heuristiques pratiques pour concevoir des systèmes plus faciles à comprendre, à tester et à faire évoluer. Elles s'appuient sur des défaillances réelles et sur les refontes qui les ont corrigées.
