---
title: "Initialiseurs désignés en C++ : quand `[0]{}` devient une lambda"
date: 2026-11-10T00:01:00.000Z
source: Arthur O'Dwyer
link: https://quuxplusone.github.io/blog/2026/11/10/designated-or-lambda/
tags:
  - cpp
feed_tag: contrived-examples
lang: en
level: advanced
format: article
reading_time: 3
summary: "Arthur O'Dwyer explique pourquoi le C++ standard refuse les désignateurs d'éléments de tableau à la C99 : une syntaxe comme `[zero]{}` peut se lire soit comme un initialiseur, soit comme une lambda."
takeaways:
  - Les initialiseurs désignés de C++20 n'acceptent ni l'ordre libre, ni le mélange avec des initialiseurs non désignés, ni les désignateurs de tableau.
  - En C++, `[zero]{}` est une lambda valide qui capture `zero`, ce qui rend la syntaxe `[0]{}` de Clang ambiguë.
  - GCC n'accepte la syntaxe sans `=` qu'en mode C, où elle est déclarée obsolète, alors que Clang l'accepte aussi en C++ avec des avertissements.
  - Aucun compilateur n'accepte un indice de désignateur calculé à l'exécution, comme `[argc] = 1`.
status: published
featured: false
edition: 2026-W47
---
C++20, via la proposition P0329, a introduit une forme volontairement limitée des initialiseurs désignés (`.field = 42`). Contrairement au C, il n'accepte ni l'ordre libre des champs, ni le mélange d'initialiseurs désignés et non désignés, ni les désignateurs d'éléments de tableau comme `[0] = 42`. Arthur O'Dwyer montre pourquoi ces derniers posent problème, en renvoyant à la section [diff.dcl] du standard et au comportement de Clang.

```cpp
bool oldstyle() {
  #define ZERO 0
  std::any a[3] = {
    [ZERO] {},
  };
  return a[0].has_value(); // false
}

bool modernized() {
  constexpr int zero = 0;
  std::any b[3] = {
    [zero] {},
  };
  return b[0].has_value(); // true
}
```

Dans un tableau de `std::any`, l'écriture `{ [ZERO] {}, }`, où `ZERO` est une macro valant 0, est traitée par Clang comme un désignateur C99 : « initialiser l'élément 0 avec cette liste vide d'arguments de constructeur ». `a[0].has_value()` renvoie donc `false`. En revanche, avec une constante locale `constexpr int zero = 0`, `{ [zero] {}, }` est à juste titre lu comme une lambda qui capture `zero`, sans liste de paramètres et avec un corps vide. C'est du C++ valide que tous les compilateurs doivent analyser ainsi, et `b[0].has_value()` renvoie `true`. L'auteur regrette au passage que l'omission de la liste de paramètres d'une lambda soit autorisée.

GCC, à l'origine de l'extension des initialiseurs désignés, n'a jamais accepté en mode C++ la syntaxe sans `=` de Clang. En mode C seulement, il accepte `int i[3] = { [1] 42 };`, que son manuel déclare « obsolète depuis GCC 2.5 ». Clang accepte cette syntaxe en C comme en C++. En C++, il émet `-Wgnu-designator` pour l'élément scalaire mais, depuis Clang 10, ne signale qu'un `-Wc99-designator` pour l'élément de type structure `[1] {}`.

En note, l'auteur rappelle que, même en C où l'ordre libre est permis, aucun compilateur à sa connaissance n'accepte un indice connu seulement à l'exécution. `int a[5] = { [argc] = 1 };` n'équivaut pas à `int a[5] = {}; a[argc] = 1;` : GCC et Clang le rejettent tous les deux.
