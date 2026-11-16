---
title: "CppCon 2026 vue par Sandor Dargo : C++26, horizon C++29 et omniprésence de l'IA"
date: 2026-11-13T21:47:39.000Z
source: isocpp.org
link: https://isocpp.org//blog/2026/11/trip-report-cppcon-2026-sandor-dargo
tags:
  - cpp
lang: en
level: intermediate
format: article
reading_time: 13
image: https://isocpp.org/files/img/cppcon-2026-muc-airport-alex-and-me.jpg
summary: Dans son compte rendu de CppCon 2026, Sandor Dargo revient sur ses trois conférences et sur les sessions marquantes de l'édition, centrée sur l'IA, C++26 et les travaux en cours pour C++29.
takeaways:
  - C++29 se dessine autour des profiles (sécurité par analyse statique) et d'extensions des contrats
  - La réflexion de C++26 permet déjà des bibliothèques comme `rjk::duck`, des interfaces sans héritage
  - "Les abstractions « à coût nul » ne le sont pas toujours : mesurer les chemins critiques"
  - "IA et C++ : rester critique face au code généré, donner des objectifs et des contextes courts"
status: published
featured: false
edition: 2026-W47
---
Pour sa deuxième participation comme orateur à CppCon, à Denver, Sandor Dargo dresse un bilan chiffré : 24 sessions suivies, 3 conférences données et 21 billets LinkedIn. Il insiste toutefois sur le côté humain de la plus grande conférence C++ du monde, où les retrouvailles commencent avant même d'arriver sur place : une escale à Munich l'a fait tomber sur Alex Dathskovsky, un autre orateur qui prenait le même vol que lui.

Il a présenté « The Clocks of C++ », un tour des différentes horloges de la bibliothèque standard et des critères pour choisir la bonne, y compris pour faire correspondre des horodatages entre systèmes. Sa deuxième conférence, « The Real Story of C++26 », laisse de côté les contrats, la réflexion et sender/receiver pour se concentrer sur les petites nouveautés qui rendent le code correct plus facile à écrire et les erreurs plus faciles à détecter, à la compilation comme à l'exécution. Une troisième session, en format libre et non enregistrée, portait sur la perte du « flow » chez les développeurs à l'ère de l'IA.

Parmi les sessions qu'il a suivies : Timur Doumler a présenté six extensions des contrats envisagées pour C++29, comme les assertions sur les fonctions virtuelles, les messages de violation personnalisés ou les assertions implicites contre le comportement indéfini. Bjarne Stroustrup a présenté les profiles, attendus pour C++29 sous la forme d'un cadre d'analyse statique dont les deux premiers profils visent les erreurs d'initialisation et les pointeurs pendants. Ryan Keane a été ovationné pour sa bibliothèque header-only `rjk::duck`, qui génère des vtables par réflexion afin que des types sans lien d'héritage partagent une interface. Côté performance, Steve Sorkin a montré le coût réel de certaines abstractions, comme `std::function` face aux templates ou les ranges face aux boucles écrites à la main. Alex Dathskovsky a rappelé que la notation grand O ignore les caches et la prédiction de branchement, et Matt Godbolt a expliqué que l'inlining vaut surtout pour les optimisations qu'il rend possibles ensuite.

L'IA était l'autre grand thème de l'édition. Peter Muldoon a mis en garde contre la « reddition cognitive » face au code généré et contre l'épuisement des développeurs qui le relisent. Andrei Alexandrescu, qui remplaçait Herb Sutter, malade, pour la keynote de clôture, conseille d'utiliser au moins deux agents de fournisseurs différents. Jody Hagins recommande de confier à chaque appel un objectif unique et précis, avec un contexte réduit.
