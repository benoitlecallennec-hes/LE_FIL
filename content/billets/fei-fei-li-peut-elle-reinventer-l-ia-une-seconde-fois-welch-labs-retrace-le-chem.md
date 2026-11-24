---
title: Fei-Fei Li peut-elle réinventer l'IA une seconde fois ? Welch Labs retrace le chemin d'ImageNet aux modèles de monde
date: 2026-11-20T22:11:52.000Z
source: Welch Labs
link: https://www.youtube.com/watch?v=hORDMRY_bZ0
tags:
  - ia
lang: en
level: intermediate
format: video
reading_time: 40
image: https://i1.ytimg.com/vi/hORDMRY_bZ0/hqdefault.jpg
summary: "Welch Labs part d'ImageNet et de la question des données en robotique pour expliquer les techniques qui sous-tendent les modèles de monde génératifs : NeRF, Gaussian splats, prédiction de nouvelles vues, diffusion bidirectionnelle, teacher forcing et diffusion forcing."
takeaways:
  - Le manque de données reste le principal obstacle en robotique, et la vidéo se demande ce que pourrait être son équivalent d'ImageNet.
  - Pour représenter des scènes en 3D, la vidéo passe des NeRF aux Gaussian splats, puis à la génération de nouvelles vues avec contrôle de caméra (CameraCtrl).
  - Diffusion bidirectionnelle, teacher forcing, diffusion forcing et modality forcing y sont comparés comme stratégies d'entraînement des modèles de monde.
missing:
  - contenu de la vidéo (seuls le titre, les chapitres et les références sont connus)
  - date de publication
status: rejected
draft: true
featured: false
---
La vidéo de Welch Labs revient d'abord sur le travail de longue haleine qu'a représenté ImageNet. Elle aborde ensuite le problème des données en robotique, illustré par une ancienne vidéo du laboratoire de Fei-Fei Li et par le Behavior Project de Stanford. La question qui traverse la vidéo : un jeu de données pourrait-il devenir « l'ImageNet de la robotique » ?

Côté représentation 3D, la vidéo présente les NeRF, avec en exemple le survol d'un appartement, puis la concurrence qui s'est installée avec Sora d'OpenAI et Genie de Google DeepMind. Elle montre aussi les Gaussian splats produits avec Marble. Elle passe ensuite à la prédiction de nouvelles vues et à CameraCtrl, une méthode de contrôle de la caméra pour la génération de vidéo à partir de texte, construite sur Stable Video Diffusion. Elle y relève au passage un problème de taille.

La partie la plus technique compare plusieurs façons d'entraîner les modèles génératifs : la diffusion bidirectionnelle, le teacher forcing, le diffusion forcing, puis le modality forcing et Atlas. Elle traite aussi de l'entraînement à partir de zéro. La vidéo se termine sur le rêve de Fei-Fei Li, tel qu'elle le raconte dans son livre *The Worlds I See*.
