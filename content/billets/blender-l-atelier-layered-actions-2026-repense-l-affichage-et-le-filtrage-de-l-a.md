---
title: "Blender : l'atelier Layered Actions 2026 repense l'affichage et le filtrage de l'animation"
date: 2026-11-03T10:37:30.000Z
source: Blender Developers Blog
link: https://code.blender.org/2026/11/layered-actions-workshop-september-2026/
tags:
  - 3d
feed_tag: Animation/Rigging
lang: en
level: intermediate
format: article
reading_time: 9
image: https://code.blender.org/wp-content/uploads/2026/11/animation_workshop_2025-edited-1024x633.webp?x95207
summary: "La veille de la Blender Conference 2026, le module Animation & Rigging a passé une journée sur les Layered Actions. Il en ressort des propositions pour les éditeurs d'animation : canaux organisés autour de l'Action, épinglage, groupes personnalisés et gestion des Actions directement depuis le Dope Sheet et le Graph Editor."
takeaways:
  - Le système de filtrage d'animation, vieux de 17 ans (Blender 2.50), est en cours de remplacement et sert de fondation à la suite.
  - Les canaux d'animation seraient organisés par Action plutôt que par objet, avec les données partagées affichées une seule fois et une option « Minimal Grouping ».
  - "Nouvelles commandes d'affichage proposées : épinglage de canaux par éditeur et groupes personnalisés définis par les riggers ou les animateurs."
  - L'assignation des Actions et des Slots se ferait depuis le N-panel de tous les éditeurs d'animation, et l'Action Editor deviendrait indépendant de la sélection.
status: published
featured: false
edition: 2026-W47
---
Le module Animation & Rigging de Blender a tenu un atelier d'une journée la veille de la Blender Conference 2026. Une dizaine de participants y ont travaillé, dont Sybren Stüvel, Christoph Lendenfeld et Nathan Vegdahl. Le développement se concentre pour l'instant sur le remplacement du système de « filtrage d'animation » (#163813). Ce système décide quelles données d'animation les éditeurs affichent et sur lesquelles les outils agissent. Il date de Blender 2.50, il y a 17 ans. Son remplaçant, beaucoup plus souple, a obligé l'équipe à se demander quoi faire de cette souplesse.

L'équipe est partie d'un cas simple : trois cubes partagent un même mesh et sont animés par une seule Action, et un matériau partagé possède sa propre Action. Dans Blender 5.2, le Dope Sheet répète le mesh et le matériau sous chaque objet, ce qui compte leurs clés plusieurs fois. Le nom de l'Action n'apparaît que pour les objets, et de nombreux canaux de résumé occupent de la place verticale. La proposition est de ne plus calquer l'Outliner et d'adopter une vue centrée sur l'animation. L'Action devient l'élément de premier niveau, ce qui s'appuie sur les Slotted Actions introduites dans Blender 4.4. Les données partagées n'apparaissent qu'une fois, au même niveau que les objets. Une option « Minimal Grouping » masque les résumés et les groupes qui ne contiennent qu'un élément. Seuls les canaux qui ont des clés sont affichés.

Pour le Graph Editor, l'objectif est d'afficher les mêmes canaux que le Dope Sheet quand on passe de l'un à l'autre, soit en ajoutant des modes, soit en fusionnant les deux éditeurs. Les courbes seraient empilées verticalement par calque, et les clés hors courbes, comme celles de Grease Pencil, pourraient s'y ajouter. L'atelier propose aussi d'épingler des canaux : un canal épinglé reste visible quels que soient les filtres, de façon temporaire et propre à chaque éditeur. Il propose enfin des groupes de canaux personnalisés, prédéfinis par les riggers ou créés à la volée par les animateurs. L'idée de départ est que les animateurs raisonnent selon la structure du rig, pas selon les structures de données de Blender.

Aujourd'hui, seul l'Action Editor permet d'assigner des Actions, et il ne montre que celle de l'objet actif. L'atelier propose deux changements. D'abord, placer l'assignation des Actions et des Slots dans le panneau latéral (N-panel) de tous les éditeurs d'animation, avec des outils pour échanger, fusionner ou scinder des Actions et déplacer des Slots, y compris par glisser-déposer. Ensuite, faire de l'Action Editor un outil d'édition de n'importe quelle Action, indépendant de la sélection. L'implémentation sera progressive : le nouveau filtrage d'animation passe en premier, puis le travail sur les Layered Actions (#154504) reprendra.
