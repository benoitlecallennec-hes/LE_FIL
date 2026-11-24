---
title: Contrôle temps réel, simulation CUDA et visualisation Unreal Engine dans un même cadre C++23
date: 2026-11-16T15:00:32.000Z
source: CppCon
link: https://www.youtube.com/watch?v=CUQbzKDPp_I
tags:
  - cpp
lang: en
level: advanced
format: video
image: https://i4.ytimg.com/vi/CUQbzKDPp_I/hqdefault.jpg
summary: À la CppCon, Kevin Gomez suit de bout en bout un système de contrôle temps réel écrit en C++23. Le même cadre logiciel couvre les boucles de contrôle déterministes sur microcontrôleur, la simulation accélérée par CUDA, le pilotage depuis un navigateur et le rendu 3D en direct dans Unreal Engine.
takeaways:
  - Un seul runtime C++23 couvre à la fois les hôtes POSIX et les microcontrôleurs bare-metal.
  - Les boucles de contrôle sont déterministes et n'allouent pas de mémoire.
  - Les noyaux CUDA s'exécutent dans les boucles ordonnancées sans les bloquer.
  - La télémétrie est diffusée en temps réel vers Unreal Engine pour la visualisation 3D.
missing:
  - contenu de la vidéo
  - transparents de la présentation
  - date de publication
status: selected
draft: true
featured: false
edition: 2026-W48
---
Kevin Gomez présente un cadre temps réel open source en C++23. Il ordonnance des boucles de contrôle déterministes aussi bien sur des hôtes POSIX que sur des microcontrôleurs bare-metal. L'idée centrale est une architecture d'exécution unifiée : contrôle embarqué, simulation, diagnostic, opérations et visualisation partagent le même runtime au lieu de se répartir entre des piles logicielles séparées.

L'exposé aborde l'ordonnancement déterministe, une conception temps réel sans allocation et le déploiement multiplateforme. Il montre aussi comment intégrer des noyaux de calcul CUDA directement dans les boucles de contrôle ordonnancées sans bloquer leur exécution. Il présente l'outillage qui sert à valider le comportement temps réel déterministe, ainsi que les techniques de diffusion en temps réel de la télémétrie et des données de capteurs entre la couche de simulation et celle de visualisation.

Deux démonstrations en direct illustrent la chaîne. Dans la première, un quadricoptère simulé suit une trajectoire programmée en s'appuyant sur un retour lidar. La seconde est une simulation d'avion complète, avec pilote automatique en boucle fermée, dynamique moteur et turbulence atmosphérique. Les deux passent par le même cadre temps réel et sont rendues en direct dans Unreal Engine. L'exposé vise les personnes qui développent des logiciels de vol, de la robotique, des contrôleurs industriels ou des infrastructures de simulation.
