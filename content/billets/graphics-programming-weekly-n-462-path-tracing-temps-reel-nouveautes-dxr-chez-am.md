---
title: "Graphics Programming Weekly n° 462 : path tracing temps réel, nouveautés DXR chez AMD et upscaling neuronal de textures"
date: 2026-11-08T04:00:00.000Z
source: Graphics Programming Weekly
link: https://www.jendrikillner.com/post/graphics-programming-weekly-issue-462/
tags:
  - 3d
lang: en
level: intermediate
format: article
reading_time: 3
image: https://www.jendrikillner.com/img/posts/graphics-programming-weekly-462/Whitepapers.png
summary: Cette semaine, la sélection de Jendrik Illner va d'un livre blanc NVIDIA sur le path tracing en jeu à la prise en charge par AMD des nouveautés DXR de l'Agility SDK. Elle passe aussi par un upscaler neuronal de textures, le bilan de douze ans de développement du moteur Spartan et plusieurs outils open source.
takeaways:
  - "Path tracing en jeu : budgéter les rayons par signal plutôt que par pixel, et combiner des rebonds courts avec un cache de radiance et ReSTIR"
  - "Les pilotes AMD pour RDNA prennent en charge l'Agility SDK 1.722-preview : Clustered Geometry, Partitioned TLAS et construction de BVH pilotée par le GPU"
  - Un seul MLP généraliste intégré à l'échantillonnage suffit pour agrandir les textures, sans réentraînement par asset et avec moins de VRAM
  - "Nouveaux outils open source : `gputaskman` pour suivre l'usage du GPU par processus, et la bibliothèque Splines qui produit des courbes de Bézier"
status: published
featured: false
edition: 2026-W47
---
Le numéro s'ouvre sur un livre blanc de NVIDIA consacré au path tracing temps réel dans les moteurs de jeu. Il conseille de répartir le budget de rayons par signal plutôt que par pixel, et de combiner des rebonds peu profonds avec un cache de radiance et ReSTIR. Il traite aussi de la mise à jour des structures d'accélération, de la gestion des fireflies et du réglage du lancement des rayons (ray dispatch) en lien avec le débruitage et l'upscaling. Côté API, AMD annonce que ses pilotes pour GPU RDNA prennent en charge les DirectX 12 Agility SDK 1.620 (version finale) et 1.722-preview. La préversion apporte des fonctions DXR comme Clustered Geometry, Partitioned TLAS et la construction de structures d'accélération pilotée par le GPU (GPU-indirect). La version finale ajoute les DirectX Dump Files, des marqueurs PIX étendus et l'intégration d'Advanced Shader Delivery.

Côté recherche, un article sur arXiv propose un upscaling neuronal léger des textures, intégré directement à l'étape d'échantillonnage. Un seul MLP généraliste, entraîné sur des patchs de textures variés, évite de réentraîner un réseau pour chaque asset et de modifier le pipeline d'assets. Il réduit le stockage sur disque et la VRAM consommée, avec une exécution sans divergence sur le GPU. Dans un registre plus appliqué, l'épisode 34 de la série « Around The World » améliore le ciel, la diffusion atmosphérique et les nuages d'un jeu de voile stylisé. L'auteur y vérifie la diffusion multiple, absente de POV-Ray, avec un ray tracer maison écrit en Rust, puis précalcule de nouvelles LUT de nuages. Il partage aussi des astuces de shaders Godot : des normales de nuages orientées vers le haut pour l'éclairage indirect, et un ajustement du vecteur de vue pour l'horizon d'une petite planète.

Panos Karabelas revient sur douze années de développement en solo du moteur open source Spartan. Celui-ci atteint aujourd'hui un état qu'il juge « livrable », avec path tracing temps réel, Lua et scripting visuel. Il en tire des leçons sur la persévérance dans l'open source et sur l'effet de ce projet sur sa carrière. Côté outils, la bibliothèque Splines rassemble des algorithmes de splines interpolantes (cubique C2, cubique G2, Hobby, kappa-Curves d'Adobe et une variante maison S9, G2-continue et à faible courbure) qui produisent des courbes de Bézier cubiques ou quadratiques. `gputaskman` est une interface en terminal qui affiche la mémoire GPU et l'activité de calcul par processus. Il fonctionne sous Windows (télémétrie NVIDIA/AMD et activité WDDM pour Intel), sous Linux, sous WSL et sur Apple Silicon.

Deux vidéos complètent la sélection. L'épisode 34 de « Vulkan For Beginners » poursuit le rendu hors écran : une interface ImGui active ou désactive les passes de rendu à l'exécution, et la vidéo montre comment gérer les descriptor sets, les pipeline layouts et la préparation de chaque image d'une passe à l'autre. L'autre vidéo explique le Thompson sampling, une heuristique qui arbitre entre exploration et exploitation dans les problèmes de bandits manchots, en tirant dans des lois Beta pour modéliser les probabilités de gain.
