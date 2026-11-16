---
title: "C++23 : construire des conteneurs à partir de ranges avec `from_range` et `ranges::to`"
date: 2026-11-09T16:29:58.000Z
source: C++ Weekly
link: https://www.youtube.com/watch?v=KPKYN6A1VcQ
tags:
  - cpp
lang: en
level: intermediate
format: video
image: https://i4.ytimg.com/vi/KPKYN6A1VcQ/hqdefault.jpg
summary: "L'épisode 558 de C++ Weekly, présenté par Jason Turner, porte sur deux ajouts de C++23 qui relient les ranges aux conteneurs de la bibliothèque standard : l'étiquette `from_range` et la fonction `std::ranges::to`."
takeaways:
  - C++23 ajoute `std::from_range`, une étiquette pour construire un conteneur standard directement à partir d'un range.
  - "`std::ranges::to` convertit un range ou une chaîne de vues en conteneur concret."
  - Les détails de l'épisode sont dans l'issue 533 du dépôt GitHub `lefticus/cpp_weekly`.
status: published
featured: false
edition: 2026-W47
---
Dans l'épisode 558 de sa série hebdomadaire C++ Weekly, Jason Turner traite de deux nouveautés de C++23 qui concernent les ranges : `std::from_range` et `std::ranges::to`. La description de la vidéo ne détaille pas le contenu de l'épisode. Elle renvoie à un ticket GitHub du dépôt `cpp_weekly` (issue 533) pour les détails.

Pour situer le sujet : C++23 a ajouté aux conteneurs standard des constructeurs qui prennent l'étiquette `std::from_range` suivie d'un range. Il a aussi introduit `std::ranges::to`, qui convertit un range, par exemple le résultat d'une chaîne de vues, en conteneur concret. Ces deux outils comblent un manque de C++20, où transformer une vue en conteneur demandait de passer par des itérateurs.

Le reste de la description fait la promotion des ateliers C++ Best Practices de l'auteur (Aurora, Kongsberg, Breda), de son sponsor Hudson River Trading et de ses livres, dont C++23 Best Practices.
