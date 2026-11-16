---
title: "C++26 : `std::constant_wrapper` fait traverser les fonctions aux valeurs constantes"
date: 2026-11-03T23:00:00.000Z
source: sandordargo.com
link: https://www.sandordargo.com/blog/2026/11/04/cpp26-constant-wrapper
tags:
  - cpp
feed_tag: dev
lang: en
level: advanced
format: article
reading_time: 9
summary: Sandor Dargo présente `std::constant_wrapper` et son raccourci `std::cw<V>`, ajoutés en C++26. Une valeur connue à la compilation passe en argument d'une fonction et reste une expression constante dans son corps.
takeaways:
  - Un paramètre de fonction n'est jamais une expression constante, même dans une fonction `constexpr`. `std::constant_wrapper` contourne la limite en mettant la valeur dans le type.
  - "`std::cw<V>` fonctionne avec tout type structurel : entiers, flottants, chaînes littérales."
  - Les opérateurs appliqués à des `constant_wrapper` produisent de nouveaux `constant_wrapper` calculés à la compilation. `operator()` et `operator[]` viennent de P3978R3.
  - "`function_ref` utilise désormais `constant_wrapper` à la place de `std::nontype` (P3948R1)."
status: published
featured: false
edition: 2026-W47
---
En C++, un paramètre de fonction n'est jamais une expression constante, même dans une fonction `constexpr` appelée avec une valeur connue à la compilation. On ne peut donc pas l'utiliser comme argument de template. Jusqu'ici, il fallait passer par des contournements : paramètres de template, types étiquettes ou `std::integral_constant`, qui sont verbeux ou limités.

`std::constant_wrapper` encode la valeur dans le type lui-même, sous forme de paramètre de template non-type. Le type enveloppé peut être n'importe quel type structurel. Le wrapper se convertit implicitement vers la valeur qu'il contient. Il accepte donc les entiers, mais aussi les flottants et, grâce aux paramètres de template de type classe introduits en C++20, les chaînes littérales. À l'appel, il suffit d'écrire `std::cw<1>`, et la fonction appelée peut se servir de cette valeur comme d'une constante :

```cpp
constexpr int x = get_element(std::cw<1>); // x == 20
```

Les opérateurs arithmétiques, de comparaison, logiques et bit à bit appliqués à des `constant_wrapper` renvoient un nouveau `constant_wrapper`, calculé à la compilation. `&&` et `||` ne sont surchargés que si le type de la valeur d'un opérande n'est pas convertible en `bool`. La proposition P3978R3 ajoute des `operator()` et `operator[]` qui déballent les valeurs. Le résultat ne reste enveloppé que si le wrapper et tous les arguments sont eux-mêmes des `constant_wrapper`.

L'exemple qui ouvre la proposition vient de `std::submdspan`, où les indices de découpage connus à la compilation deviennent plus lisibles. Dans la bibliothèque standard, P3948R1 se sert aussi de `constant_wrapper` dans `function_ref`, à la place du mécanisme séparé `std::nontype`.
