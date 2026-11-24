---
title: "Pilote AMD Adrenalin 26.11.2 WHQL : OpenGL 4.6, Vulkan 1.4.359 et dix correctifs"
date: 2026-11-13T12:49:14.000Z
source: Geeks3D
link: https://www.geeks3d.com/20261113/amd-radeon-adrenalin-26-11-x-graphics-driver/
tags:
  - 3d
feed_tag: opengl
lang: en
level: beginner
format: article
reading_time: 4
image: https://www.geeks3d.com/wp-content/uploads/2026/08/radeon-graphics-02.jpg
summary: AMD publie le pilote graphique Adrenalin 26.11.2 WHQL pour les cartes Radeon sous Windows 10/11. Il ajoute la prise en charge de ROCm 10.2 et de deux nouveaux jeux, et corrige dix bogues. Côté API, il expose OpenGL 4.6 avec 326 extensions et Vulkan 1.4.359 avec 224 extensions de périphérique.
takeaways:
  - Adrenalin 26.11.2 WHQL prend en charge ROCm 10.2, Crimson Desert Enhanced (DLC Charting the Unknown) et Phantom Blade Zero
  - Dix correctifs, dont une fuite mémoire et des écritures disque excessives au repos
  - OpenGL 4.6 avec 326 extensions, Vulkan 1.4.359 avec 224 extensions de périphérique, OpenCL 2.1
  - "Cartes concernées : Radeon RX 5000 à RX 9000, pour Windows 10/11 en 64 bits"
missing:
  - balise og:image non vérifiée
status: published
featured: false
edition: 2026-W48
notions:
  - Allocation dynamique
---
Geeks3D suit la série de pilotes AMD Radeon Adrenalin 26.11.x. La version 26.11.2 WHQL (pilote daté du 18 novembre 2026, OpenCL build 3686.0) succède à la bêta 26.11.1 du 13 novembre. Elle prend en charge ROCm 10.2, ainsi que deux jeux : le DLC Charting the Unknown de Crimson Desert Enhanced et Phantom Blade Zero. Les cartes concernées vont des Radeon RX 5000 aux RX 9070/9060/9050 et à l'AI PRO R9000, avec leurs déclinaisons pour portables.

Les dix correctifs portent notamment sur la désactivation possible de Smart Access Memory après installation sous Windows 10 et sur un plantage au premier lancement de Resident Evil Requiem après réinitialisation du cache de shaders. S'y ajoutent une corruption visuelle des cheveux dans Phantom Blade Zero sur RX 9000, des écritures excessives sur le disque système au repos et une fuite mémoire qui ralentissait de plus en plus le lancement des applications. D'autres corrections concernent des plantages ou des timeouts du pilote dans War Thunder, dans OBS Studio pendant un stream, après l'AMD Cleanup Utility ou au lancement d'une application depuis un chemin long. La bêta 26.11.1 corrigeait déjà des écritures disque excessives au repos et pendant le déplacement ou le redimensionnement des fenêtres.

Sur une Radeon RX 9070 XT, Geeks3D relève OpenGL 4.6 (contexte `4.6.0 Compatibility Profile Context 26.11.2.261114`, GLSL 4.60) avec 326 extensions. Le pilote expose aussi Vulkan 1.4.359 avec 224 extensions de périphérique et OpenCL 2.1 AMD-APP avec 24 extensions, le GPU `gfx1201` se déclarant en OpenCL 2.0.
