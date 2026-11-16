---
title: "Daniel Lemire : l'open source comme méthode de recherche"
date: 2026-11-04T12:48:24.000Z
source: lemire.me
link: https://lemire.me/blog/2026/11/04/open-source-as-a-research-method/
tags:
  - autres
lang: en
level: beginner
format: article
reading_time: 5
image: https://lemire.me/blog/wp-content/uploads/2026/11/seriq-cover-150x150.jpg
summary: Daniel Lemire raconte comment `simdjson`, l'algorithme Eisel-Lemire et `simdutf` sont nés d'abord sous forme de code publié et utilisé, avant les articles scientifiques. Pour lui, les utilisateurs sont le vrai moteur de la recherche.
takeaways:
  - Pour Lemire, le code publié et adopté passe avant l'article, qui sert surtout de vitrine.
  - "L'optimisation de `simdjson` a fait apparaître des goulots successifs : l'algorithme Eisel-Lemire (intégré en C++ via `fast_float`), puis `simdutf`."
  - Les utilisateurs open source font une revue par les pairs plus exigeante que les comités scientifiques.
  - Selon l'auteur, l'IA augmente le nombre de contributions aux projets open source au lieu de le réduire.
status: published
featured: false
edition: 2026-W47
---
Daniel Lemire conteste l'image d'une innovation qui irait en ligne droite, du professeur qui publie à l'ingénieur qui conçoit puis au client qui consomme. Selon lui, c'est la demande qui vient d'abord. Il cite Michael Stonebraker, pour qui la recherche en bases de données a perdu ses « clients du monde réel » et dérive faute d'eux. Il reprend aussi la formule de Fred Brooks : le scientifique construit pour étudier, l'ingénieur étudie pour construire. Dans son cas, construire est sa manière d'étudier.

En 2019, le JSON était partout et son analyse plafonnait à quelques centaines de mégaoctets par seconde. Son équipe a écrit `simdjson`, qui atteint aujourd'hui plusieurs gigaoctets par seconde (environ 4,8 Go/s sur `twitter.json` avec un Xeon récent). Le code est venu en premier, suivi d'un billet de blog, d'une publication sur GitHub et sur les réseaux sociaux, puis d'une adoption par l'industrie. L'article du VLDB Journal a surtout servi de vitrine. Chaque gain a ensuite révélé un nouveau goulot d'étranglement. D'abord la conversion des chaînes en flottants, qui a donné l'algorithme Eisel-Lemire, exact et rapide, désormais intégré aux bibliothèques standard de Rust, Go, .NET et C++ (via `fast_float`). Puis la validation UTF-8, ramenée à moins d'une instruction par octet, qui est devenue la bibliothèque `simdutf` utilisée par Node.js, Bun, Deno et les navigateurs.

L'open source lui fournit des utilisateurs qui arrivent avec des rapports de bugs, des entrées malveillantes, du matériel exotique et des charges de travail imprévues. Les pull requests et les revues de code sont souvent plus exigeantes que les comités de programme, parce que ceux qui dépendent du code le lisent plus attentivement qu'un relecteur qui survole un article. Avec David Patterson, il estime que le meilleur signe précoce de réussite est de voir des non-chercheurs essayer d'utiliser ses idées. Quant à l'IA, elle ne rend pas l'open source moins utile : ses projets n'ont jamais eu autant de contributeurs, car ces outils aident à lire un code inconnu et à proposer un correctif.

Lemire a présenté ces idées le 29 octobre 2026 à Montréal, comme l'un des quatre invités du lancement de SERIQ. Ce nouveau centre interuniversitaire de recherche en génie logiciel est soutenu par l'Université de Montréal, Polytechnique Montréal, McGill et l'ÉTS, et prend la suite des rencontres SEMTL.
