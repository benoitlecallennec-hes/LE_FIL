---
title: Des calculs flottants reproductibles bit à bit sans sacrifier la vitesse du matériel
date: 2026-11-20T15:07:19.000Z
source: CppCon
link: https://www.youtube.com/watch?v=-5yzXMyyV98
tags:
  - cpp
lang: en
level: advanced
format: video
image: https://i2.ytimg.com/vi/-5yzXMyyV98/hqdefault.jpg
summary: À la CppCon, Andrew Drakeford montre que `reduce` et `scan` peuvent donner exactement les mêmes bits quels que soient les threads, la largeur SIMD ou l'architecture, à condition de spécifier l'expression calculée plutôt que l'ordre d'exécution.
takeaways:
  - La reproductibilité vient du fait de spécifier l'expression calculée, pas de figer l'ordonnancement.
  - Un `scan` reproductible doit garantir tous les préfixes, pas seulement la somme finale.
  - FMA, dénormaux, environnement flottant et bibliothèques mathématiques sont les sources habituelles de divergence.
  - Les propositions P4016R0 et P4229R0 portent cette approche dans la normalisation C++.
missing:
  - contenu de la vidéo
  - chiffres des mesures de performance
  - durée de la vidéo
  - date de publication
status: selected
draft: true
featured: false
edition: 2026-W48
---
Le flottant a la réputation de trahir : il suffit de changer le nombre de threads, la largeur des vecteurs, les options du compilateur, l'arbre de réduction ou l'architecture cible pour que les bits de poids faible bougent. Les algorithmes parallèles aggravent le problème, car la norme spécifie souvent l'opération, mais pas l'expression numérique dont le résultat doit être reproduit. Andrew Drakeford présente des implémentations reproductibles et déterministes de `reduce` et de `scan` qui se comportent mieux face aux erreurs sur des charges flottantes hostiles, et qui égalent ou battent les implémentations classiques de la bibliothèque standard sur des charges réalistes.

L'astuce ne consiste pas à figer l'ordonnancement, mais à nommer l'expression calculée. C'est l'idée de « reproductibilité par reproduction du calcul », développée dans les propositions de normalisation P4016R0 et P4229R0. Une fois l'expression choisie, l'implémentation reste libre d'employer SIMD, threads, découpage en blocs ou en tuiles et stratégies propres à la plateforme : changer le nombre de threads, la largeur vectorielle ou le découpage ne modifie plus le résultat en silence. Le `scan` est plus difficile que le `reduce`, car il expose chaque préfixe et pas seulement la valeur finale ; des contrats d'expression et d'observation rendent ces préfixes reproductibles sans revenir à un ordre séquentiel lent.

L'exposé descend ensuite sous la couche des algorithmes, là où les bits s'échappent d'habitude : contraction en FMA, nombres dénormaux, choix de l'environnement flottant, approximations des bibliothèques mathématiques et fonctions transcendantes vectorisées. Il montre des primitives vectorisées reproductibles, fonctions transcendantes comprises, dont la vitesse est comparable à celle des bibliothèques mathématiques vectorielles établies, tout en respectant un contrat numérique commun à toutes les plateformes.

Pour finir, toute la pile est mise à l'épreuve sur un pipeline numérique hétérogène réparti entre x86-64, Apple Silicon et CUDA, avec des données volontairement hostiles : forte cancellation et états intermédiaires fragiles. Le but est de reproduire bit pour bit le calcul spécifié, y compris les mêmes défaillances intermédiaires, et pas seulement la même réponse finale, sur tous les CPU, GPU et chaînes de compilation.
