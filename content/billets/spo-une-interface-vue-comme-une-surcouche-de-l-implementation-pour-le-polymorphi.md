---
title: "SPO : une interface vue comme une surcouche de l'implémentation pour le polymorphisme statique"
date: 2026-11-11T00:59:48.000Z
source: isocpp.org
link: https://isocpp.org//blog/2026/11/subobject-polymorphic-overlay-pattern-felix-olivier-dumas
tags:
  - cpp
lang: en
level: advanced
format: article
summary: "Félix-Olivier Dumas présente le Subobject Polymorphic Overlay Pattern (SPO), un idiome expérimental de polymorphisme statique en C++. Il inverse le schéma habituel : ce n'est plus l'implémentation qui hérite de l'interface, c'est l'interface qui se pose comme une vue sur une implémentation indépendante."
takeaways:
  - "Le SPO inverse le contrat du polymorphisme statique : l'interface est une vue sélective sur une implémentation indépendante, au lieu d'en être la classe de base."
  - Il vise un polymorphisme résolu à la compilation, sans surcoût à l'exécution.
  - Il se présente comme une alternative au CRTP et à `std::variant`, dont il éviterait certains défauts.
  - L'auteur insiste sur le caractère expérimental de l'idiome et sur ses compromis.
status: published
featured: false
edition: 2026-W47
---
Dans un article publié sur Medium et relayé par isocpp.org, Félix-Olivier Dumas décrit le Subobject Polymorphic Overlay Pattern (SPO). Il le présente comme un idiome d'architecture expérimental pour le polymorphisme statique en C++. Il part d'un constat : le polymorphisme en C++ se divise en deux grandes familles, statique et dynamique. Elles partagent la même philosophie, mais diffèrent fortement dans leur mise en œuvre comme dans leur usage.

Le SPO inverse le modèle de contrat habituel du polymorphisme statique. Avec le CRTP, l'implémentation hérite d'une interface paramétrée par le type dérivé. Avec le SPO, l'interface devient une vue propre et sélective posée sur une implémentation qui reste indépendante. L'implémentation n'a donc plus à connaître l'interface par laquelle on l'utilise.

Selon l'auteur, on obtient ainsi un polymorphisme résolu à la compilation, sans surcoût à l'exécution. Il éviterait certains défauts habituels des approches fondées sur le CRTP ou sur `std::variant`. L'article reste explicite sur les compromis de la technique et sur son caractère expérimental.

La page d'isocpp.org ne contient qu'un résumé et un court extrait. L'article complet, hébergé sur Medium, n'a pas pu être consulté : ce résumé ne détaille donc pas le mécanisme exact ni les exemples de code.
