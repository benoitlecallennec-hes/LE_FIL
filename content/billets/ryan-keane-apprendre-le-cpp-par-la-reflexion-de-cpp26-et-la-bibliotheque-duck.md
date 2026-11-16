---
title: "Ryan Keane : apprendre le C++ par la réflexion de C++26 et la bibliothèque duck"
date: 2026-11-02T16:29:50.000Z
source: C++ Weekly
link: https://www.youtube.com/watch?v=d--qkwcF5vM
tags:
  - cpp
lang: en
level: advanced
format: video
image: https://i1.ytimg.com/vi/d--qkwcF5vM/hqdefault.jpg
summary: Dans l'épisode 414 de CppCast, diffusé aussi comme C++ Weekly 557, Ryan Keane présente `duck`, une bibliothèque qui s'appuie sur la réflexion de C++26 pour obtenir un comportement proche du polymorphisme sans interfaces intrusives.
takeaways:
  - "`duck` utilise la réflexion de C++26 pour proposer des interfaces non intrusives, avec un comportement proche du polymorphisme."
  - Ryan Keane, étudiant en deuxième année, a appris le C++ en explorant la réflexion de C++26.
  - Il collabore à la proposition P4148, qui porte sur le sous-typage structurel en C++.
  - "Le code de la bibliothèque est publié sur GitHub : `RyanJK5/rjk-duck`."
status: rejected
draft: true
featured: false
---
Mathieu et Jason reçoivent Ryan Keane, étudiant en deuxième année d'informatique au Rensselaer Polytechnic Institute. Il a créé `duck`, une bibliothèque C++ de réflexion destinée à définir des interfaces non intrusives : elle permet d'obtenir un comportement proche du polymorphisme grâce à la réflexion, sans que les types concernés aient à être modifiés pour s'y conformer.

L'épisode retrace aussi la façon dont Ryan Keane a appris le C++ : il est entré dans le langage en explorant les mécanismes de réflexion introduits par C++26.

Après un billet de blog consacré au fonctionnement interne de la bibliothèque, il a été invité à parler à la CppCon. Il participe désormais à la proposition P4148, « Structural Subtyping for C++ », qui porte sur le sous-typage structurel en C++. Le code de la bibliothèque est publié sur GitHub (dépôt `RyanJK5/rjk-duck`).
