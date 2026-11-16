---
title: "C++ : `= default` et `{}` ne donnent pas le même constructeur, et GCC 15 le fait payer cher"
date: 2026-11-02T12:48:59.000Z
source: lemire.me
link: https://lemire.me/blog/2026/11/02/default-and-braces-are-not-the-same-constructor/
tags:
  - cpp
lang: en
level: advanced
format: article
reading_time: 4
image: https://lemire.me/blog/wp-content/uploads/2026/11/cppinit-cover-150x150.jpg
summary: Daniel Lemire montre qu'un constructeur par défaut vide écrit `span() = default` ou `span() {}` ne se comporte pas pareil, et qu'avec GCC 15 et GCC 16 la version `= default` peut être plus de quatre fois plus lente.
takeaways:
  - "`span() = default` garde le type trivial : `span s{}` met alors tout l'objet à zéro, padding compris."
  - "`span() {}` est fourni par l'utilisateur : ni `span s;` ni `span s{}` ne mettent les membres à zéro."
  - Avec GCC 15.2 et 16.2 (`-O2`), la version `= default` est plus de quatre fois plus lente dans le test de l'auteur ; GCC 14.3 et clang 21 ne montrent pas d'écart.
status: published
featured: false
edition: 2026-W47
---
Il y a deux façons d'écrire un constructeur par défaut vide : `span() = default` ou `span() {}`. Quand `= default` apparaît dès la première déclaration, le type reste trivial. `span() {}` est un constructeur fourni par l'utilisateur, donc le type n'est plus trivial. La différence se voit à l'initialisation. Avec `= default`, `span s;` laisse les membres non initialisés, alors que `span s{}` met tout l'objet à zéro, padding compris, sans appeler le constructeur. Avec `{}`, `span s;` et `span s{}` appellent tous deux le constructeur vide, que le compilateur peut supprimer, et les membres restent non initialisés.

```cpp
struct span {
  const char * p;
  int len;
  const char * end;
  span() = default; // or span() {}
  span(const char * s, int n) : p(s), len(n), end(s + n) {}
};
```

Pour son exemple, l'auteur enveloppe ce `span` dans une structure `box` dont le constructeur `box() : s() {}` value-initialise le membre. Une boucle affecte `box()` quand `i == n` et `box(span(s, i + 1))` dans les autres cas. Le chemin `box()` ne s'exécute qu'une fois, à la sortie de la boucle, et sa valeur n'est jamais passée à `consume`. Avec `span() {}`, GCC 16 (`-O2 -march=x86-64`) écrit les trois champs directement là où `consume` les attend. Avec `span() = default`, il range le pointeur de fin directement dans la `box`, mais fait passer le pointeur de début et la longueur par un temporaire, dont il recopie ensuite les 16 premiers octets (`movdqa`/`movaps`).

Le test mesure le meilleur de cinq passes de 100 millions de spans, sur un cœur d'un Intel Xeon Gold 6548N (Emerald Rapids). GCC 14.3 et clang 21 donnent environ 1,73 à 1,75 ns par span dans les deux cas. Avec GCC 15.2 et GCC 16.2, la version `= default` monte à 7,75 et 8,04 ns par span, contre 1,73 ns pour `{}`, soit plus de quatre fois plus lent. Le code du test est publié par l'auteur.
