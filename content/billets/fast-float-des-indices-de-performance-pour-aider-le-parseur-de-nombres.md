---
title: "fast_float : des indices de performance pour aider le parseur de nombres"
date: 2026-11-10T12:44:09.000Z
source: lemire.me
link: https://lemire.me/blog/2026/11/10/telling-your-number-parser-what-to-expect/
tags:
  - autres
lang: en
level: advanced
format: article
reading_time: 5
image: https://lemire.me/blog/wp-content/uploads/2026/11/tunefastfloat-feature-150x150.jpg
summary: Daniel Lemire ajoute à fast_float, la bibliothèque de conversion texte vers flottant utilisée notamment dans la bibliothèque standard C++ de GCC, trois « indices » passés en paramètre template. Ils réduisent les erreurs de prédiction de branchement quand on connaît la forme de ses données, et peuvent plus que doubler le débit.
takeaways:
  - "fast_float accepte trois indices combinables, passés en paramètre template : `unpredictable_sign`, `short_numbers` et `assume_round_to_nearest`."
  - Ils suppriment des branchements mal prédits et vont jusqu'à plus que doubler le débit (153 → 345 millions de nombres/s sur Apple M4 Max).
  - "Un indice inadapté aux données ralentit le parseur : il faut choisir la combinaison selon les données et l'architecture."
  - Seul `assume_round_to_nearest` peut fausser des résultats, si le programme change le mode d'arrondi.
status: published
featured: false
edition: 2026-W47
---
Analyser un nombre comme `-731.51` oblige le parseur à prendre plusieurs décisions : y a-t-il un signe, combien de chiffres, où est le point décimal, le processeur arrondit-il dans le mode par défaut ? Chacune correspond à un branchement. Le processeur parie sur son issue. Quand il se trompe, il jette le travail déjà fait, ce qui coûte 15 à 20 cycles. Si les données sont homogènes, il devine juste. Mais dans une colonne de gains et de pertes, le signe est aléatoire et il se trompe sur environ la moitié des nombres.

fast_float accepte donc maintenant des indices passés en argument template à `from_chars`, `from_chars_advanced` et `integer_times_pow10`, par exemple `from_chars<performance_hints::unpredictable_sign>(...)`. Ils se combinent avec l'opérateur `|`. `unpredictable_sign` traite le signe sans branchement. `short_numbers` convient quand chaque plage contient un seul nombre, le plus souvent de 8 caractères au maximum : le parseur lit alors les 8 caractères d'un coup avec quelques opérations bit à bit, sans branchement sur le nombre de chiffres ni sur la position du point. `assume_round_to_nearest` supprime la vérification du mode d'arrondi. Les indices ne changent jamais les résultats, sauf si l'on promet `assume_round_to_nearest` alors qu'une partie du programme modifie le mode d'arrondi. Sans indice, le comportement reste le même qu'avant.

```cpp
using fast_float::performance_hints;
double value;
auto answer = fast_float::from_chars<performance_hints::unpredictable_sign>(
    input.data(), input.data() + input.size(), value);
```

Lemire a mesuré quatre jeux de 100 000 nombres sur un Intel Xeon Gold 6338 (Ice Lake, GCC 15) et un Apple M4 Max (Apple clang 17). Pour des variations de prix courtes à signe aléatoire, le débit passe de 73 à 105 millions de nombres par seconde sur Intel, et de 153 à 345 millions sur Apple avec les trois indices. Sans indice, Intel se trompait de prédiction sur 60 % des nombres ; avec les trois, ces erreurs disparaissent. Pour des soldes de comptes plus longs, `unpredictable_sign` avec `assume_round_to_nearest` donne 61 → 82 millions sur Intel et 132 → 199 millions sur Apple. Ajouter `short_numbers` dégrade le résultat sur Intel (70 millions). Pour des relevés de capteurs positifs de formes variées, `short_numbers` est l'indice clé : 67 → 92 millions sur Intel, 123 → 250 millions sur Apple. `unpredictable_sign` y est nuisible. Pour des prix réguliers de forme identique, ne rien indiquer reste le meilleur choix sur Intel (152 millions, contre 104 avec `short_numbers`), alors que `short_numbers` fait passer Apple de 251 à 355 millions.

Un indice remplace des branchements peu coûteux par des instructions supplémentaires. Il n'est donc rentable que si le processeur se serait trompé, et il peut ralentir le code dans le cas contraire. Clang 21 sur Intel donne des résultats similaires. Les indices sont proposés dans la pull request #434 du dépôt fast_float.
