---
title: Hugo 0.170.1 corrige une régression de performance liée à `return` dans les partials
date: 2026-11-17T17:08:14.000Z
source: Hugo News
link: https://github.com/gohugoio/hugo/releases/tag/v0.170.1
tags:
  - autres
lang: en
level: intermediate
format: article
reading_time: 1
summary: Ce correctif de Hugo règle surtout une régression de performance arrivée avec la version 0.166.0, qui avait amélioré le mot-clé `return` dans les templates `partial`. Il ajoute aussi les options `fgColor` et `bgColor` à `images.QR`.
takeaways:
  - Corrige une régression de performance due au mot-clé `return` dans les `partial`, présente depuis Hugo 0.166.0
  - Seuls les gros sites qui utilisent `return` étaient concernés
  - "`images.QR` et le shortcode `qr` acceptent désormais `fgColor` et `bgColor`"
  - La dépendance `github.com/bep/imagemeta` passe de la v1.1.1 à la v1.1.2
status: rejected
draft: true
featured: false
---
La version 0.170.1 du générateur de sites statiques Hugo sort d'abord pour corriger une régression de performance. Celle-ci date de la version 0.166.0, qui avait amélioré le mot-clé `return` dans les templates `partial`. Selon les notes de version, seuls les gros sites qui utilisent `return` étaient touchés.

Le correctif se trouve dans `tpl/tplimpl` : Hugo n'ajoute plus le contexte de fichier aux erreurs sentinelles qui servent à signaler le retour d'un partial (ticket #15604).

Cette version ajoute aussi une fonction : la fonction `images.QR` et le shortcode `qr` acceptent les options `fgColor` et `bgColor`, qui règlent la couleur de premier plan et la couleur de fond du code QR généré (ticket #15600). Le reste est de l'entretien : la dépendance `github.com/bep/imagemeta` passe de la v1.1.1 à la v1.1.2, les commentaires et la gestion des arguments du shortcode `vimeo` sont nettoyés, le fichier de test `hugolib/integrationtest_builder_test.go` est supprimé et les tests sont adaptés pour ne plus produire d'erreurs de dépréciation.
