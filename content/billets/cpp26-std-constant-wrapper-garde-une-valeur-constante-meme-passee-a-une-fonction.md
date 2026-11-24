---
title: "C++26 : `std::constant_wrapper` garde une valeur constante même passée à une fonction"
date: 2026-11-15T22:36:05.000Z
source: isocpp.org
link: https://isocpp.org//blog/2026/11/cpp26-stdconstant-wrapper-sandor-dargo
tags:
  - cpp
lang: en
level: advanced
format: article
reading_time: 9
image: https://isocpp.org/files/img/SANDOR_DARGO_ROUND.JPG
summary: Sandor Dargo présente `std::constant_wrapper` et son raccourci `std::cw`, ajoutés par C++26. Ils transportent une valeur connue à la compilation à travers un appel de fonction sans qu'elle cesse d'être une expression constante.
takeaways:
  - Un paramètre de fonction n'est jamais une expression constante, même dans une fonction `consteval`.
  - "`std::cw<X>` enveloppe une valeur de n'importe quel type structurel (pas seulement un entier), et le code appelé la lit par `T::value`."
  - "Les opérateurs renvoient un nouveau `constant_wrapper` : `cw<10> + cw<3>` vaut `constant_wrapper<13>`."
  - Déclaré dans `<utility>` (P2781R9 et P3978R3), il remplace `std::integral_constant` dans `submdspan` et `function_ref`.
status: selected
draft: true
featured: false
edition: 2026-W48
---
Une limite de C++ gêne tous ceux qui s'y heurtent : un paramètre de fonction n'est pas une expression constante, même dans une fonction `constexpr` ou `consteval`. Le corps de la fonction est vérifié une seule fois, et non pour chaque valeur d'argument. On contourne cela avec des paramètres template, des types étiquettes ou `std::integral_constant`. Ces solutions sont verbeuses et se combinent mal. `integral_constant` récupère la valeur par `T::value`, mais il n'a pas d'opérateurs arithmétiques. L'article précise d'ailleurs qu'il n'est pas limité aux entiers : `integral_constant<double, 2.5>` est valide en C++20.

`std::constant_wrapper<X>` enveloppe une valeur de n'importe quel type structurel dans un paramètre template non-type. `std::cw<X>` en produit une instance sans écrire le type. L'enveloppe se convertit implicitement vers la valeur qu'elle contient. Le code appelé lit `T::value` comme une expression constante, par exemple pour construire un `ScaledArray<double Factor>` à partir de `std::cw<2.5>`. L'outil est déclaré dans `<utility>`. P2781R9 le plaçait dans `<type_traits>`, mais P3978R3 l'a déplacé, car ses opérateurs `operator()` et `operator[]` reposent sur INVOKE.

Les opérateurs binaires sont surchargés : arithmétiques, comparaisons, logiques et bit à bit. Chacun renvoie un nouveau `constant_wrapper`, et une comparaison donne un `bool` enveloppé. Les opérateurs unaires `-`, `~`, `!`, `++` et `--` fonctionnent aussi. `++` et `--` ne modifient rien et renvoient une nouvelle enveloppe. `&&` et `||` ne sont surchargés que si l'un des opérandes n'est pas convertible en `bool`.

```cpp
constexpr auto sum  = a + b;   // constant_wrapper<13>
```

P3978R3 ajoute aussi des appels et des indexations qui « déballent » la valeur, sur le modèle de `std::reference_wrapper`. Le résultat ne reste enveloppé que si l'enveloppe et tous les arguments sont des `constant_wrapper`. Par exemple, `std::cw<iota>[std::cw<1>]` donne `constant_wrapper<1>`. La proposition part de `std::submdspan`, où `std::cw<N>` remplace `std::integral_constant<size_t, N>{}` pour les indices de tranche connus à la compilation. Une chaîne littérale comme `std::cw<"hello">` est stockée dans un tableau de caractères de taille fixe. Enfin, P3948R1 remplace par `constant_wrapper` le mécanisme séparé de `function_ref`.
