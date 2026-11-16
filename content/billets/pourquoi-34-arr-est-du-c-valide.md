---
title: Pourquoi `34[arr]` est du C valide
date: 2026-11-10T15:00:31.000Z
source: Jacob Sorber
link: https://www.youtube.com/watch?v=-0CFZI3gRfc
tags:
  - c
lang: en
level: intermediate
format: video
image: https://i2.ytimg.com/vi/-0CFZI3gRfc/hqdefault.jpg
summary: "Jacob Sorber se penche sur une curiosité du C : l'écriture `34[arr]`, qui inverse le tableau et l'indice tout en restant légale. Il l'aborde à travers l'opérateur d'indexation, qu'il range parmi les plus étranges du langage."
takeaways:
  - "`34[arr]` est une expression C valide, équivalente à `arr[34]`."
  - En C, l'indexation se ramène à une addition sur un pointeur suivie d'un déréférencement.
  - Cette curiosité illustre le lien étroit entre tableaux et pointeurs en C.
status: published
featured: false
edition: 2026-W47
---
Dans cette vidéo, Jacob Sorber part d'une expression qui surprend souvent : `34[arr]`. On y trouve un entier devant les crochets et le nom du tableau à l'intérieur, l'inverse de la forme habituelle `arr[34]`. Il pose une question simple : comment le langage C peut-il accepter une telle écriture ?

La vidéo s'intéresse à l'opérateur d'indexation `[]`, présenté comme l'un des opérateurs les plus curieux du C. La norme définit `E1[E2]` comme équivalent à `*((E1)+(E2))`. L'addition étant commutative, `arr[34]` et `34[arr]` désignent donc le même élément.

Le sujet touche au lien étroit entre pointeurs et tableaux en C. L'auteur renvoie d'ailleurs à une vidéo plus ancienne de sa chaîne sur ce thème (« Pointers and Arrays ») pour approfondir. Le code source est réservé aux personnes qui soutiennent la chaîne sur Patreon.
