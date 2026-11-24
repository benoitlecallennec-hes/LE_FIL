---
title: "Muru : le créateur de mod_wsgi écrit avec l'IA un serveur Rust pour les applications Python"
date: 2026-11-21T23:56:28.000Z
source: Simon Willison
link: https://simonwillison.net/2026/Nov/21/muru/
tags:
  - ia
feed_tag: ai
lang: en
level: intermediate
format: article
reading_time: 2
summary: Graham Dumpleton, à l'origine de `mod_wsgi` il y a vingt ans, développe Muru avec l'aide de l'IA. C'est un serveur écrit en Rust pour faire tourner des applications Python `ASGI` et `WSGI`, en développement comme en production. Il raconte le projet au fil de l'eau dans une longue série de billets.
takeaways:
  - Muru est un serveur en Rust, écrit avec l'aide de l'IA, pour les applications Python `WSGI` et `ASGI`.
  - Son auteur, Graham Dumpleton, est le créateur de `mod_wsgi`.
  - "Le développement est raconté au jour le jour : workers, Kubernetes, mise à l'échelle jusqu'à zéro, barre d'outils de traçage."
  - Pour l'auteur, l'IA écrit beaucoup de code, mais c'est son expérience qui décide de ce qui est assez bon.
missing:
  - image principale de la page
status: published
featured: false
edition: 2026-W48
---
Graham Dumpleton a lancé `mod_wsgi` il y a vingt ans. Depuis, il cherche la meilleure façon de servir des applications web Python en production. Son nouveau projet, Muru, est un serveur écrit en Rust avec l'aide de l'IA. Il veut offrir les meilleurs outils possibles pour exécuter des applications Python `ASGI` et `WSGI`, en développement comme en production.

Comme il l'avait fait pour wrapture, il publie un compte rendu très détaillé du projet pendant qu'il y travaille, et cette écriture fait partie de sa méthode de développement. Simon Willison recense dix-neuf billets parus en trois semaines, du 4 au 22 novembre 2026. Ils portent notamment sur ces sujets :

- les vingt ans de `mod_wsgi` ;
- l'intérieur des workers et le principe « importer une fois, forker plusieurs fois » ;
- `HTTPS` en développement ;
- la cohabitation de `WSGI` et `ASGI` ;
- les mises en production sans redémarrage et les déploiements canari sans service mesh ;
- le fonctionnement en conteneur et avec Kubernetes ;
- la mise à l'échelle à partir de ce que rapportent les workers, jusqu'à zéro instance ;
- une barre d'outils de diagnostic avec traçage sur chaque page ;
- les cas où les imports paresseux sont utiles.

Simon Willison recommande en particulier le billet « AI as a lifestyle choice ». Graham Dumpleton y écrit que l'IA fait peut-être une grande partie du travail en surface, mais que c'est toujours à lui de décider ce qui n'est pas encore assez bon. Ses vingt ans d'erreurs avec `mod_wsgi` lui servent pour cela. Il dit être prêt à fournir le travail nécessaire pour peaufiner Muru jusqu'à ce qu'il lui fasse confiance, et que les autres puissent lui faire confiance aussi.
