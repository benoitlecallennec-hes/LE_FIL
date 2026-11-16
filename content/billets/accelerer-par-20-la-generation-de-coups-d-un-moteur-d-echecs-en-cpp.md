---
title: Accélérer par 20 la génération de coups d'un moteur d'échecs en C++
date: 2026-11-14T15:00:10.000Z
source: CppCon
link: https://www.youtube.com/shorts/A38GVPFJSjM
tags:
  - cpp
lang: en
level: intermediate
format: short
reading_time: 1
image: https://i2.ytimg.com/vi/A38GVPFJSjM/hqdefault.jpg
summary: Dans ce format court publié par CppCon, un développeur explique comment il a rendu la génération de coups de son moteur d'échecs vingt fois plus rapide, notamment grâce aux bitboards.
takeaways:
  - La génération de coups est un goulot d'étranglement classique des moteurs d'échecs
  - Les bitboards sont au cœur des optimisations présentées
  - Il faut profiler et mesurer avant d'optimiser
  - Le gain annoncé est un facteur 20 sur la génération de coups
status: published
featured: false
edition: 2026-W47
---
CppCon publie une courte vidéo où un développeur raconte comment il a rendu la génération de coups de son moteur d'échecs vingt fois plus rapide. La génération de coups consiste à énumérer tous les coups légaux d'une position. C'est l'une des opérations les plus sollicitées dans un moteur d'échecs, et donc l'un de ses principaux goulots d'étranglement.

D'après la description, la vidéo porte sur les bitboards et l'architecture du moteur. Elle identifie les goulots d'étranglement de la génération de coups et présente les algorithmes retenus pour gagner en vitesse. Elle aborde aussi le profilage et la mesure des performances, ainsi que des techniques pour écrire du C++ plus efficace dans les applications où la vitesse compte.

La vidéo s'adresse aux personnes qui s'intéressent au développement de jeux, à l'optimisation des performances ou à la programmation bas niveau, qu'elles écrivent leur propre moteur ou non. La description ne précise pas les techniques employées une par une.
