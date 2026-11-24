---
title: "RoboTrack : Ai2 publie un banc d'essai et 15 millions de trajectoires synthétiques pour le suivi de points en manipulation robotique"
date: 2026-11-23T17:20:37.000Z
source: Hugging Face Blog
link: https://huggingface.co/blog/allenai/robotrack
tags:
  - ia
lang: en
level: advanced
format: article
image: https://www.datocms-assets.com/64837/1795407244-robotrack-blog-draft-google-docs-image-1.png
summary: "Ai2 publie RoboTrack, un ensemble qui comprend trois éléments : un banc d'essai annoté à la main sur de vraies vidéos de manipulation, un jeu d'entraînement synthétique de 15 millions de trajectoires et deux trackers affinés, RoboTracker et RoboTracker-Warp."
takeaways:
  - "RoboTrack-Bench : 307 vidéos réelles de manipulation et 2 522 trajectoires annotées à la main, sur lesquelles les trackers actuels perdent de 3,0 à 6,6 points d'AJ par rapport à RoboTAP."
  - "RoboTrack-15M : 15 millions de trajectoires générées avec MolmoSpaces et MuJoCo. Il n'est devenu utile qu'une fois les points concentrés sur le robot et les objets manipulés."
  - RoboTracker-Warp passe de 63,9 à 66,0 AJ sur RoboTrack-Bench. Ses poids sont réservés à un usage non commercial.
  - Sur une tâche réelle de pipetage, le suivi de points réussit 17 essais sur 20, contre 8 pour les masques SAM3 et 5 pour le flot optique RAFT.
status: published
featured: false
edition: 2026-W48
---
Le suivi de points consiste à suivre un point choisi d'une image à l'autre d'une vidéo, y compris quand il est masqué puis réapparaît. Les robots se servent de ces trajectoires pour apprendre à partir de démonstrations, planifier et modéliser la réaction des objets à leurs actions. Pour Ai2, les trackers existants obtiennent de bons scores sur les bancs d'essai robotiques actuels, mais ces bancs ne reflètent pas forcément la variété des scènes, des points de vue et des interactions que les robots rencontrent en pratique. RoboTrack-Bench rassemble 307 vidéos issues de DROID, Ego-Exo4D, MolmoAct 2 et EgoVerse. On y trouve divers robots et des mains humaines, des scènes encombrées, des vues à la première et à la troisième personne. Le banc contient 2 522 trajectoires vérifiées à la main, chacune classée en corps du robot ou de l'humain, objet manipulé ou objet statique.

RoboTrack-15M est produit en simulation avec MolmoSpaces, l'environnement de simulation d'Ai2, étendu avec MuJoCo. Un bras simulé saisit des objets du quotidien et la scène est filmée par une caméra au poignet et une caméra à la base. Les points sont placés à partir de la géométrie, des poses et des caméras du simulateur, sans annotation humaine. Le jeu compte 3 000 vidéos tournées dans 300 maisons simulées, avec 5 000 trajectoires par vidéo. La première version n'apportait aucun gain : les points tirés au hasard dans toute l'image tombaient surtout sur les murs ou les meubles et restaient presque immobiles en vue extérieure. Les auteurs ont donc limité l'échantillonnage au robot et aux objets manipulés, puis déplacé les points de la base fixe vers le bras mobile. Ces changements ont rapproché les données des vraies vidéos et porté le score à 63,0 AJ, soit 1,5 point au-dessus de la configuration témoin entraînée sur Kubric seul.

Tous les trackers évalués (CoTracker3, BootsTAPNext, AllTracker, CoWTracker) perdent entre 3,0 et 6,6 points d'Average Jaccard (AJ) sur RoboTrack-Bench par rapport à RoboTAP. RoboTracker, qui affine AllTracker (environ 17 M de paramètres), passe de 62,6 à 63,4 AJ. RoboTracker-Warp, qui affine CoWTracker (environ 1 milliard de paramètres), passe de 63,9 à 66,0 AJ. Leurs résultats sur les vidéos généralistes restent au niveau des modèles de base. RoboTracker-Warp obtient le meilleur score sur quatre des cinq bancs d'essai ; seul DAVIS lui échappe.

Pour un essai sur un vrai robot, une seule démonstration téléopérée sert à guider un pipetage de laboratoire d'un flacon à l'autre, sur 20 dispositions de flacons. Le suivi de points avec RoboTracker réussit 17 transferts complets sur 20. Les masques d'objets de SAM3 en réussissent 8 et le flot optique RAFT 5. Les modèles, les jeux de données, le code et le rapport technique sont publiés ; les poids de RoboTracker-Warp sont réservés à un usage non commercial.
