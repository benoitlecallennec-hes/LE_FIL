---
title: "Meeting C++ Blogroll n° 552 : réflexion C++26, `std::constant_wrapper` et pièges de performance"
date: 2026-11-06T17:15:45.000Z
source: meetingcpp.com
link: https://www.meetingcpp.com/blog/blogroll/items/Meeting-Cpp-weekly-Blogroll-552.html
tags:
  - cpp
lang: en
level: intermediate
format: article
reading_time: 2
summary: "Comme chaque semaine, Jens Weller rassemble des liens vers des articles, des vidéos et des rencontres de groupes d'utilisateurs C++. Au menu cette fois : la réflexion et `std::constant_wrapper` en C++26, plusieurs billets de Daniel Lemire sur la performance, la sécurité du C++ et la sortie de CLion 2026.3."
takeaways:
  - "C++26 est très présent : réflexion (CppCast), `std::constant_wrapper` (Sandor Dargo) et profils (Stroustrup, Guy Davidson)."
  - "Daniel Lemire publie trois billets : le *store forwarding*, `= default` face à `{}`, et la division signée sur 32 bits."
  - "Côté outils : CLion 2026.3 et Botan 3.14.0 / 3.14.1."
  - "Vidéo à signaler : les informations de débogage Windows (CodeView, PDB) par Hans Wennborg."
status: rejected
draft: true
featured: false
---
La Blogroll n° 552 de Meeting C++ (6 novembre 2026) liste, sans commentaire, les publications de la semaine issues de ses flux RSS. Côté langage, Barry Revzin part des séquences de jetons (*token sequences*) pour aboutir à `operator for`, et Sandor Dargo présente `std::constant_wrapper`, nouveauté de C++26. Modernes C++ publie la deuxième partie de sa série sur la sécurité du C++, sous le titre « Modern C++ Is a Mindset, Not a Standard ». CppCast consacre un épisode à l'apprentissage du C++ avec la réflexion de C++26.

Daniel Lemire signe trois billets orientés performance et sémantique : un piège de performance lié à l'échec du *store forwarding*, la différence entre un constructeur `= default` et un constructeur `{}`, et une façon de faire mieux que le compilateur pour la division signée sur 32 bits. On trouve aussi la sortie de CLion 2026.3, les versions 3.14.0 et 3.14.1 de la bibliothèque cryptographique Botan, et un article de Jeff Amstutz sur le rendu portable et la place qu'y occupe ANARI.

La rubrique vidéo signale la conférence de Bjarne Stroustrup « Profiles for Simplicity and Guarantees » à la CppCon 2026, un entretien avec Guy Davidson sur C++26 et les profils, et l'exposé de Hans Wennborg sur les informations de débogage Windows (formats CodeView et PDB). La page annonce enfin les rencontres de groupes d'utilisateurs de la semaine du 9 novembre, à Austin, York, Dresde et Irvine, ainsi qu'en ligne.
