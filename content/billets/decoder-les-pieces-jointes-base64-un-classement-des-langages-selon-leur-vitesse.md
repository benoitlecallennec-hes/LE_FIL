---
title: "Décoder les pièces jointes base64 : un classement des langages selon leur vitesse, avec un facteur trente entre les extrêmes"
date: 2026-11-20T12:37:52.000Z
source: lemire.me
link: https://lemire.me/blog/2026/11/20/decoding-base64-email-attachments/
tags:
  - autres
lang: en
level: intermediate
format: article
reading_time: 3
image: https://lemire.me/blog/wp-content/uploads/2026/11/mimespeed-cover-150x150.jpg
summary: Daniel Lemire mesure la vitesse de décodage du base64 MIME (lignes de 76 caractères terminées par CRLF) dans onze langages et environnements. Les six premiers s'appuient tous sur la bibliothèque simdutf, et sa pull request rend le `base64_decode` de PHP 28 fois plus rapide.
takeaways:
  - Beaucoup de décodeurs base64 rapides passent sur un chemin lent dès qu'ils rencontrent une fin de ligne, ce qui pénalise le MIME et le PEM.
  - Les six systèmes les plus rapides (8,4 à 18,4 Go/s) utilisent simdutf, qui retire les CRLF dans sa boucle SIMD en compactant les blocs concernés.
  - Go (`base64.StdEncoding.Decode`), Java (`Base64.getMimeDecoder()`) et Zig restent entre 0,85 et 1,6 Go/s.
  - Avec la pull request de Lemire, le `base64_decode` de PHP passe de 0,61 à 17,1 Go/s sur du base64 MIME.
status: published
featured: false
edition: 2026-W48
---
Les pièces jointes des courriels sont encodées en base64, tout comme les certificats PEM. Dans les deux cas, le texte est découpé en lignes : 76 caractères pour le courriel, 64 pour PEM. Ignorer les fins de ligne paraît trivial, mais beaucoup de décodeurs rapides ne traitent que du base64 continu. Dès qu'ils rencontrent un saut de ligne, ils passent sur un chemin lent.

Pour mesurer l'effet, Daniel Lemire génère un mégaoctet d'octets aléatoires et l'encode en base64 avec des lignes de 76 caractères terminées par CRLF, soit 1,37 Mo de texte. Il le décode ensuite avec la fonction standard ou la plus courante de chaque langage, puis vérifie que le résultat est correct. La machine est un Intel Xeon Gold 6548N (Emerald Rapids) sous Linux, avec un seul cœur réservé au test. Les vitesses sont exprimées en Go d'entrée par seconde. Chaque valeur est la médiane de trois exécutions, et chaque exécution garde le meilleur de dix lots chronométrés. En tête, C++ (GCC 16) avec simdutf 9.4 et `simdutf::base64_to_binary` atteint 18,4 Go/s. Suivent PHP 8.7-dev avec sa pull request (17,1), Python 3.15 avec `fastpybase64.b64decode` (16,4), Bun 1.4.4 (14,2) et Node.js 26.13 (13,3) avec `Buffer.from(s, 'base64')`, puis Deno 2.9.8 avec `Uint8Array.fromBase64(s)` (8,4). Viennent ensuite .NET 11 avec `Base64.DecodeFromUtf8` (4,9), Go 1.27.4 avec `base64.StdEncoding.Decode` (1,6), Java 27 avec `Base64.getMimeDecoder().decode` (1,2), Zig 0.17.1 avec le décodeur `std.base64` qui ignore `\r\n` (0,85) et, en dernier, PHP 8.5.12 avec `base64_decode` (0,61). L'écart entre le premier et le dernier atteint un facteur trente.

Les six premiers reposent tous sur simdutf. Cette bibliothèque retire les fins de ligne dans sa boucle vectorielle. Un bloc propre de 64 caractères est décodé directement. Un bloc qui contient des fins de ligne est d'abord compacté, puis décodé depuis un petit tampon. Les fins de ligne ne font donc jamais basculer le décodeur sur un chemin lent. Les détails sont publiés dans un article d'ACM TACO (Lemire, 2027).

PHP est le cas le plus frappant. La version actuelle est dernière avec 0,61 Go/s, alors qu'elle décode le base64 sans fins de ligne à plus de 20 Go/s sur la même machine. La pull request de l'auteur porte les décodeurs de simdutf dans PHP : le décodage MIME devient 28 fois plus rapide et PHP passe à la deuxième place. Le code du benchmark est disponible sur GitHub.
