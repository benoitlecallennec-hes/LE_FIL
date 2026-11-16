---
title: "Blogroll Meeting C++ n° 553 : hazard pointers en C++26, Dynamic Debugging en version finale et six failles corrigées dans Qt"
date: 2026-11-13T09:05:02.000Z
source: meetingcpp.com
link: https://www.meetingcpp.com/blog/blogroll/items/Meeting-Cpp-weekly-Blogroll-553.html
tags:
  - cpp
lang: en
level: intermediate
format: article
reading_time: 3
summary: Dans sa revue hebdomadaire du 13 novembre 2026, Jens Weller rassemble les nouvelles versions de bibliothèques et d'outils, les articles techniques de la semaine, six avis de sécurité publiés par Qt et quelques vidéos consacrées au C++26 et au C++29.
takeaways:
  - C++ Dynamic Debugging de Visual C++ est désormais en version finale, et ReSharper C++ 2026.3 ajoute l'affichage de la disposition des classes en mémoire.
  - "Six vulnérabilités ont été corrigées dans Qt (`QTextCodec`, XML, WebSocket, QDom, Multimedia/FFmpeg, HTTP) : une mise à jour est conseillée."
  - "Articles à lire en priorité : les hazard pointers de C++26 (Bancila), `co_await` avec plusieurs paramètres (Raymond Chen) et les annotations de durée de vie dans le Clang Static Analyzer."
  - Selon une vidéo de Meeting C++, le vote sur le DIS de C++26 a échoué à cause des Contracts.
status: published
featured: false
edition: 2026-W47
---
Le Meeting C++ weekly Blogroll n° 553, publié le 13 novembre 2026 par Jens Weller, commence par les nouvelles versions de la semaine. La bibliothèque de cryptographie Botan passe en 3.14.1 et la bibliothèque de formatage fmt en 12.2.1. ReSharper C++ 2026.3 ajoute une vue de la disposition des classes en mémoire et l'analyse des chaînes de format. Côté Microsoft, le blog de l'équipe Visual C++ annonce que C++ Dynamic Debugging est désormais en version finale. Il indique aussi que MSBuild est configuré automatiquement pour le serveur de langage C++ de Microsoft dans Copilot CLI.

Plusieurs articles abordent la programmation de près. Marius Bancila présente les hazard pointers introduits par C++26. Daniel Lemire explique comment indiquer à un analyseur de nombres le format d'entrée à attendre. Raymond Chen (The Old New Thing) montre comment passer plus d'un paramètre à `co_await`. Dans le cadre du Google Summer of Code 2026, le blog LLVM décrit comment le Clang Static Analyzer a appris à exploiter les annotations de durée de vie. Nicholas Frechette poursuit une série sur la représentation des transformations affines, avec une table des matières et un billet sur les matrices affines. On trouve aussi un court article de Meeting C++ sur `std::string`, un billet de mp-units sur le respect du système de build de l'utilisateur, et une réflexion de Sandor Dargo sur ce que l'IA peut apprendre de l'histoire de la photographie, des compilateurs et des métiers à tisser.

Le Qt Project a publié six avis de sécurité. Ils concernent un dépassement d'entier dans `QTextCodec`, une récursion non contrôlée dans `QXmlSimpleReader` et un épuisement de ressources lors de la poignée de main de `QWebSocket`. S'y ajoutent un déréférencement de pointeur nul dans `QDomDocument::setContent()`, une corruption mémoire dans le backend FFmpeg de Qt Multimedia et une consommation de ressources non contrôlée dans le traitement des réponses HTTP de Qt Network. Le blog Qt présente aussi une certification pour étudiants développeurs et annonce les Qt Champions 2026.

La revue liste enfin une offre d'emploi C++20/23 en robotique et IA à Brno, ainsi que les prochaines réunions de groupes d'utilisateurs C++ (Irvine, Austin, Portland). En vidéo, l'épisode 558 de C++ Weekly de Jason Turner traite de `from_range` et de `ranges::to`, apparus en C++23. Deux courtes vidéos de Meeting C++ expliquent ce que sont les profils prévus pour C++29 et pourquoi le vote sur le DIS de C++26 a échoué à cause des Contracts. SwedenCpp propose une présentation de la réflexion par Jonas Lund.
