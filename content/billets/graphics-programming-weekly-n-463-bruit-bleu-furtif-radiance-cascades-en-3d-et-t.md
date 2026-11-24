---
title: "Graphics Programming Weekly n° 463 : bruit bleu furtif, radiance cascades en 3D et textures neuronales"
date: 2026-11-15T04:00:00.000Z
source: Graphics Programming Weekly
link: https://www.jendrikillner.com/post/graphics-programming-weekly-issue-463/
tags:
  - 3d
lang: en
level: advanced
format: article
reading_time: 3
image: https://www.jendrikillner.com/img/posts/graphics-programming-weekly-463/stealthy-blue-noise.png
summary: "Au sommaire du numéro 463 : un bruit bleu sans motif régulier, une version 3D des radiance cascades, deux travaux sur les textures neuronales, la quantification `FP8`/`INT8`/`INT4` des réseaux et l'annonce de Vulkanised 2027."
takeaways:
  - Split Radiance Cascades porte les radiance cascades en 3D grâce à des sondes éparses allouées par table de hachage et à des LOD de sondes
  - Avec un filtrage anisotrope appris, la compression neuronale de textures approche la qualité des textures compressées pleine taille en utilisant un quart de la mémoire
  - La quantification linéaire des poids (`FP8`, `INT8`, `INT4`) repose sur un facteur d'échelle et un point zéro, avec une granularité par tenseur, par canal ou par groupe
  - Vulkanised 2027 aura lieu à Courtrai du 8 au 12 février, avec un tarif early-bird jusqu'au 15 janvier
missing:
  - contenu des articles et vidéos référencés (seuls les résumés de la newsletter ont été lus)
  - balise og:image non vérifiée (image du premier article retenue)
status: published
featured: false
edition: 2026-W48
---
Côté échantillonnage et éclairage, *Stealthy Blue Noise* génère des échantillons de bruit bleu qui répartissent les points de façon homogène sans former de grille régulière. La méthode règle le motif en choisissant les fréquences spatiales à supprimer, y compris dans des zones où la densité visée varie. Elle est appliquée au pointillisme (stippling) et aux surfaces sphériques, avec des motifs qui restent visuellement cohérents quand la densité change. *Split Radiance Cascades* adapte la technique des radiance cascades à l'illumination globale en 3D : des sondes éparses sont placées sur les surfaces visibles et les rayons tracés sont découpés en intervalles de distance. L'article détaille l'allocation des sondes par table de hachage, les bins de radiance directionnelle et les niveaux de détail (LOD) des sondes, qui servent à doser mémoire et qualité. Une vidéo, *The Hidden Assumptions in Every Volume Renderer*, explique pourquoi l'atténuation de la lumière dans un volume suit une exponentielle quand les particules sont réparties au hasard : chaque tranche fine retire une fraction fixe de la lumière entrante, et le nombre d'Euler permet d'écrire simplement cette absorption qui dépend de la distance.

Plusieurs travaux font appel aux réseaux de neurones. *Neural Render Proxies* entraîne, pour une scène donnée, un réseau léger qui permet un ré-éclairage interactif et différentiable de scènes statiques, à caméra et matériaux fixes. L'apprentissage part d'échantillons de chemins indépendants des sources lumineuses, ce qui sépare l'échantillonnage des chemins de l'évaluation de l'émission. *Deformable Texture Maps* propose une texture neuronale compacte, indexée par les coordonnées de texture et la déformation locale. Ces cartes sont mises en cache dans un espace de texture persistant et mises à jour seulement là où la déformation change. La démonstration porte sur des textiles en fil qui se déforment. Un autre article intègre le filtrage anisotrope directement dans la compression neuronale de textures : le décodeur apprend à adapter son filtrage à la forme de l'empreinte du pixel, à l'angle et à la distance, pour une qualité proche des textures compressées pleine taille avec un quart de la mémoire. Enfin, une vidéo sur la quantification présente la compression des poids en `FP8`, `INT8` et `INT4`. Elle décrit la quantification linéaire avec facteur d'échelle et point zéro, le choix de la granularité (tenseur, canal, groupe) et compare la quantification après entraînement à l'entraînement qui en tient compte (quantization-aware training).

Le numéro couvre aussi l'*albedo remapping* pour la diffusion sous-surfacique (SSS). L'article remonte à la physique des correspondances utilisées dans les shaders SSS courants, compare les approches et montre comment les paramètres exposés aux artistes deviennent les coefficients de diffusion dont le modèle a besoin. Dans la deuxième partie du *Intermediate's Guide To Animations*, réalisée avec Casey Muratori, on implémente le mélange d'animations dans un moteur maison. Les artefacts de mélange des rotations sont corrigés grâce au produit scalaire des quaternions, et des cycles de durées différentes sont synchronisés grâce à des timelines normalisées. La vidéo explore aussi une architecture de mélange en mode immédiat, qui accumule les couches d'animation dans un buffer de squelette partagé.

Côté événements, le Vulkanised and Real-Time Shading Symposium 2027 aura lieu à Courtrai (Belgique) du 8 au 12 février 2027. Au programme : nouveautés de Vulkan, outils de débogage, ray tracing, rendu neuronal, calcul piloté par le GPU et retours de production, avec une keynote de Ken Perlin et des sessions sur Slang, les langages de shaders, l'outillage et l'optimisation. Le tarif early-bird court jusqu'au 15 janvier.
