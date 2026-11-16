---
title: "Sovereign Tower : un RPG narratif de gestion de la Table ronde développé avec Godot"
date: 2026-11-06T14:00:00.000Z
source: godotengine.org
link: https://godotengine.org/article/godot-showcase-sovereign-tower/
tags:
  - 3d
feed_tag: Showcase
lang: en
level: beginner
format: article
reading_time: 8
image: https://godotengine.org/assets/showcase/sovereign-tower/thumbnail.jpg
summary: Dans la série « Godot Showcase », Paul Mozet et Lisa Garnier, du studio Wild Wits Games, racontent le développement de Sovereign Tower. Ils reviennent sur sa mécanique de retour dans le temps, son pipeline d'assets et leur choix de Godot.
takeaways:
  - "Le retour dans le temps repose sur deux sauvegardes par cycle et des données découpées finement, car le joueur garde ce qu'il a appris : ce n'est pas un simple retour à un état antérieur."
  - Le coût de la localisation a limité le nombre de variantes narratives.
  - Le système de scènes de Godot permet aux artistes de retoucher l'interface sans développeur. Les transform offsets de Godot 4.7 facilitent l'animation des éléments d'interface.
  - "Fonctionnalité souhaitée dans l'éditeur : plusieurs onglets de système de fichiers et d'inspecteur, verrouillables."
status: rejected
draft: true
featured: false
---
Sovereign Tower est un RPG narratif de gestion de la Table ronde, disponible sur Steam pour Windows et Linux. Le joueur incarne le nouveau souverain d'un royaume. Il recrute des chevaliers excentriques, les envoie en quête pour répondre aux doléances du peuple et des comtés voisins, et doit ménager leur ego. Quand une décision tourne mal, le démon de la tour permet de remonter le temps pour s'y prendre autrement. Paul Mozet, lead programmer, est arrivé dans le jeu vidéo par les game jams, notamment Ludum Dare, pendant ses études d'informatique. Lisa Garnier, passée par une école d'art puis par l'informatique, a été technical artist sur Aetheris et Crown Gambit avant de passer au narrative design sur ce projet.

Selon Paul Mozet, le plus gros défi technique a été le système de retour en arrière. Chaque cycle joué nécessite deux sauvegardes, une le matin et une l'après-midi, qui doivent garder l'état de tous les cycles précédents. Ce ne sont pas de simples instantanés d'état, car le souverain conserve tout ce qu'il a appris. Avec des centaines de variables narratives et de quêtes et plus d'une centaine de personnages, le système était « une parfaite usine à bugs ». L'équipe a découpé le plus finement possible les données à sauvegarder et a beaucoup testé. Quelques bugs sont malgré tout passés à la sortie et sont en cours de correction. Côté écriture, Lisa Garnier explique qu'il a fallu contenir le nombre de mots, car la localisation coûte cher pour un petit studio. L'équipe a donc renoncé à proposer des variantes de retour en arrière pour chaque interaction.

Pour l'intégration, les assets simples comme les portraits allaient directement dans Godot, tandis que les éléments d'interface plus complexes étaient confiés aux développeurs. Grâce au système de scènes, l'artiste pouvait faire des retouches visuelles sans l'aide d'un développeur. Les animations combinent AnimationPlayer et Tweens. Animer les nœuds d'interface était peu pratique jusqu'aux décalages de transformation (transform offsets) introduits dans Godot 4.7. Le studio utilisait déjà Godot pour Aetheris, et Paul Mozet y est passé depuis Unity. Tous deux apprécient sa simplicité, la prise en main de GDScript, son modèle open source et sa communauté. Paul Mozet aimerait pouvoir ouvrir plusieurs onglets de système de fichiers et d'inspecteur, et les verrouiller comme dans Unity.
