---
title: "Blender sur tablette : la feuille de route jusqu'à la sortie"
date: 2026-11-18T12:45:12.000Z
source: Blender Developers Blog
link: https://code.blender.org/2026/11/tablet-path-to-release/
tags:
  - 3d
feed_tag: General Development
lang: en
level: beginner
format: article
reading_time: 8
image: https://code.blender.org/wp-content/uploads/2026/11/f428f77b-5977-42e7-8ece-072c017d31fc-1024x576.jpeg?x95207
summary: "Après un atelier d'une semaine à Amsterdam en octobre 2026, la Blender Foundation détaille les étapes pour sortir Blender sur tablettes Android et iOS : support de plateforme, builds quotidiens, publication dans les stores, puis une interface « Compact UI » pensée pour le tactile."
takeaways:
  - "Cibles initiales : tablettes Android haut de gamme (Wacom MovinkPad Pro) et iPad récents, d'abord avec clavier et souris"
  - Builds quotidiens sur builder.blender.org et déploiement continu vers Google Play et l'App Store, malgré la question de la licence GPL
  - Opérateurs « Continue on device » et « Send to device » pour passer du desktop à la tablette
  - "« Compact UI » : une seule interface responsive, livrée par cas d'usage (portfolio, scène, sculpture, assets, storyboard)"
status: selected
draft: true
featured: false
edition: 2026-W48
---
Lancé lors d'un premier atelier en 2025, le projet tablette de Blender a fait l'objet d'un nouvel atelier d'une semaine au siège de Blender à Amsterdam en octobre 2026. Dalai Felinto et Pablo Vazquez en présentent les conclusions. Le mot d'ordre est de livrer le plus tôt possible, puis d'itérer. Les objectifs sont suivis dans la tâche « Tablet: Path to Release ». Première étape, le support initial de la plateforme : compiler Blender pour tablette doit devenir simple, avec ouverture et enregistrement des fichiers, accès en ligne et tout ce qui fonctionne déjà sur desktop. Au départ, un clavier, une souris ou un trackpad seront nécessaires. Faute de moyens, les appareils visés en premier sont les tablettes Android haut de gamme (Wacom MovinkPad Pro) et les iPad récents (iPad Pro à puce M, iPad mini 8).

Les builds tablette rejoindront ensuite l'infrastructure de builder.blender.org, avec des builds quotidiens et un déploiement continu vers Google Play, l'App Store et d'autres plateformes. À terme, les tests unitaires devront passer sur les appareils des principaux fabricants. L'équipe veut aussi s'exercer tôt à la publication dans les stores pour éviter les mauvaises surprises. Elle se dit consciente des restrictions des stores sur la licence GPL et envisagera des alternatives si nécessaire.

L'étape « Beyond Mouse and Keyboard » adapte l'interface existante au tactile et au stylet, Wacom Cintiq et Intuos compris. Elle prévoit des gestes configurables, le clavier virtuel du système, une recherche globale des opérateurs et une barre d'état aux boutons cliquables. S'y ajoutent une « modifier wheel », un menu radial flottant pour `Ctrl`/`Cmd`, `Shift` et `Alt`/`Option`, ainsi que des options propres à chaque plateforme. Pour passer d'un appareil à l'autre, deux opérateurs sont prévus : « Continue on device », qui transfère tout le fichier, et « Send to device », qui envoie une partie du fichier, par exemple un data-block avec ses dépendances.

Enfin, la « Compact UI » sera une interface responsive unique. Elle replie les groupes de boutons en popovers selon des priorités quand la place manque, et l'utilisateur peut forcer la densité voulue. Elle sera livrée par cas d'usage successifs : présentation de portfolio, assemblage de scène, sculpture (modificateur Multiresolution, Remesh, Dyntopo), création d'assets et storyboard (Timeline, Dopesheet, VSE, Grease Pencil). Une première version sortira dès qu'un premier cas d'usage sera couvert, et l'interface desktop complète restera accessible.
