---
title: "Java4Shell : `Base64.getDecoder().decode` d'OpenJDK écrit hors du tableau sur une entrée invalide"
date: 2026-11-18T19:43:03.000Z
source: lemire.me
link: https://lemire.me/blog/2026/11/18/java4shell-when-base64-decoding-leads-to-remote-code-execution/
tags:
  - autres
lang: en
level: advanced
format: article
reading_time: 4
image: https://lemire.me/blog/wp-content/uploads/2026/11/java4shell-cover-150x150.jpg
summary: "Daniel Lemire décortique la CVE-2026-114938 : sur x86-64 avec AVX-512 VBMI, une chaîne base64 invalide de 128 `A` suivis d'un `=` fait écrire le décodeur d'OpenJDK 62 octets au-delà de la fin du tableau, avant même que `IllegalArgumentException` soit levée."
takeaways:
  - "CVE-2026-114938 : `Base64.getDecoder().decode` d'OpenJDK (depuis 17.0.6) écrit 62 octets hors du tableau sur une entrée invalide, sur les machines dotées d'AVX-512 VBMI."
  - "Cause : une longueur de queue de −1, dont `bzhi` ne lit que les 8 bits de poids faible (255), d'où un masque plein et un store de 64 octets."
  - "La validation arrive trop tard : `IllegalArgumentException` est levée après l'écriture fautive."
  - Le décodeur base64 « tolérant » de Lemire, qui gère les espaces dans la boucle SIMD, est déjà livré dans les navigateurs, Node.js, Deno et Bun, et un portage vers PHP est proposé.
status: rejected
draft: true
featured: false
---
Daniel Lemire part d'un exemple minimal. La chaîne compte 128 lettres `A` et un `=`. Ce n'est pas du base64 valide, et un décodeur correct la rejette. OpenJDK écrit pourtant au-delà de la fin du tableau de sortie. Il s'agit de la CVE-2026-114938, présente dans le JDK depuis la version 17.0.6 et introduite dans OpenJDK en juin 2021. L'écriture fautive se trouve dans de l'assembleur écrit à la main. Certains affirment qu'elle ouvre une faille de sécurité majeure. L'auteur dit ne pas l'avoir vérifié, mais il se sent indirectement concerné.

```java
byte[] src = ("A".repeat(128) + "=").getBytes(StandardCharsets.US_ASCII);
Base64.getDecoder().decode(src);
```

Java calcule la taille de la sortie avant de valider l'entrée. Le `=` final compte pour un octet de remplissage, donc `decode` alloue 3 × ((129 + 3) / 4) − 1 = 98 octets. Sur x86-64 avec AVX-512 VBMI, la routine traite les 128 premiers caractères en deux blocs, selon l'algorithme de Muła et Lemire (2020), et écrit 96 octets nuls qui restent dans le tableau. Il reste un caractère, le `=`. La queue calcule (1 / 4) × 3 = 0 en arithmétique entière, puis retire un à cause du `=`. La longueur vaut alors −1.

La queue écrit avec un store masqué AVX-512, dont le masque vient de `bzhi`. Cette instruction ne lit que les 8 bits de poids faible du compte, soit 255 pour −1. Un compte supérieur à 64 ne efface aucun bit : le store écrit donc le registre entier de 64 octets à partir de l'offset 96. Deux octets tombent dans le tableau, et les 62 autres (offsets 98 à 159) au-delà de sa fin. Java lève ensuite `IllegalArgumentException` avec le message « Input byte array has wrong 4-byte ending unit », mais l'écriture a déjà eu lieu. Selon l'auteur, l'algorithme vectoriel lui-même n'est pas en cause.

Lemire note aussi que Java est en retard : son décodeur rapide s'arrête au premier espace ou saut de ligne, donc dès la première ligne d'un e-mail (coupé tous les 76 caractères) ou d'un PEM (tous les 64). Son algorithme de 2027 élimine ces caractères ignorables dans la boucle vectorielle. Ce décodeur est livré depuis mi-2025 dans Safari, Chrome, Edge, Brave, Node.js 25, Deno et Bun. Une pull request le porte vers PHP, où le décodage d'une entrée MIME de 64 Kio devient 30 fois plus rapide sur un Intel Xeon Gold 6548N et 39 fois plus rapide sur un AMD EPYC 9R45.
