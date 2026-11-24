---
title: "Téléphone ou ordinateur : planifier les contrôles d'un jeu en six étapes"
date: 2026-11-20T00:00:00.000Z
source: Unity Blog
link: https://unity.com/blog/video-game-controls-phone-or-computer
tags:
  - 3d
lang: en
level: beginner
format: article
reading_time: 5
summary: Le blog Unity propose une méthode en six étapes pour concevoir les contrôles d'un jeu destiné à la fois au tactile et au couple clavier-souris. La méthode va de la liste des actions du joueur jusqu'aux tests sur de vrais appareils, avant d'écrire sa première requête dans Unity Spark.
takeaways:
  - Définir d'abord les actions du joueur, puis leurs déclencheurs par appareil, comme le fait `Input System`.
  - Boutons tactiles d'au moins 15 mm, une alternative au clavier pour chaque action à la souris, la manette en option.
  - Vérifier la vitesse, la complexité et la durée des commandes, et déclencher les actions au relâchement.
  - Tester sur de vrais appareils en séparant entrées accidentelles et entrées ignorées.
missing:
  - adresse de l'image principale (og:image)
status: published
featured: false
edition: 2026-W48
---
Les contrôles regroupent tout ce qui permet au joueur d'agir : appuis sur l'écran, touches du clavier, boutons de manette. Le même jeu ne se joue pas de la même façon d'un appareil à l'autre. Même avec Unity Spark, la plateforme de création de jeux par IA d'Unity où l'on décrit son jeu en langage courant, il faut donc choisir les contrôles soi-même. L'article vise deux configurations principales, le tactile et le clavier-souris. La manette n'y est qu'un bonus.

La première étape consiste à partir des verbes plutôt que des boutons. On liste trois à cinq actions (se déplacer, sauter, viser, ramasser, confirmer), puis on décide quel appareil déclenche chacune. C'est le principe du package `Input System`, qui sépare une action comme `Jump` des contrôles qui la déclenchent et regroupe les actions par contexte. On associe ensuite chaque appareil aux actions. Sur téléphone, les boutons à l'écran doivent mesurer au moins 15 mm de côté selon les Xbox Accessibility Guidelines, et il faut laisser de la place aux pouces. Sur ordinateur, on garde peu de touches, on propose une option au clavier pour tout ce que fait la souris et on évite les mouvements de pointeur rapides et précis, car beaucoup de joueurs utilisent un pavé tactile. Si une manette est prise en charge, on évite d'exiger les deux sticks à la fois.

Troisième étape : désigner l'appareil principal en tête du document de conception, écrire chaque action une fois puis la décliner deux fois, et repérer les gestes sans équivalent. Un pincement n'a pas d'équivalent au clavier et le survol n'en a pas au tactile, d'où par exemple un bouton de zoom. La quatrième étape vérifie trois critères tirés des recommandations Xbox. La vitesse : les appuis rapides excluent certains joueurs. La complexité : les combinaisons de touches dans un ordre imposé posent problème même quand on peut les réassigner. La durée : maintenir une commande longtemps fatigue, et une bascule qui garde la voiture en accélération règle le problème.

La cinquième étape rend le jeu indulgent. L'action se déclenche au relâchement, ce qui permet d'annuler en glissant. On évite les gestes à plusieurs doigts pour les actions essentielles, on propose des options plus simples (tir automatique, appui n'importe où sur l'écran) et on montre les contrôles pendant la partie. Enfin, on teste sur un vrai téléphone et un vrai ordinateur en notant séparément les entrées accidentelles et les entrées ignorées, puis on change un seul contrôle à la fois avant de retester.
