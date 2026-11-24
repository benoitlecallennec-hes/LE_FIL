---
title: "C++26 : `std::inplace_vector`, un vecteur à capacité fixe qui n'alloue jamais"
date: 2026-11-18T21:45:20.000Z
source: isocpp.org
link: https://isocpp.org//blog/2026/11/cpp26-stdinplace-vector-sandor-dargo
tags:
  - cpp
lang: en
level: intermediate
format: article
reading_time: 5
image: https://isocpp.org/files/img/SANDOR_DARGO_ROUND.JPG
summary: Sandor Dargo présente `std::inplace_vector<T, N>`, le nouveau conteneur de C++26. Il offre l'interface de `std::vector`, mais ses éléments sont stockés dans l'objet lui-même, avec une capacité `N` fixée à la compilation.
takeaways:
  - "`std::inplace_vector<T, N>` (en-tête `<inplace_vector>`) : interface de vecteur, stockage dans l'objet, capacité `N` fixée à la compilation, aucune allocation."
  - "Trois façons de gérer le dépassement : `push_back` lève `std::bad_alloc`, `try_push_back` renvoie un `std::optional<T&>` sans effet en cas d'échec, `unchecked_push_back` donne un comportement indéfini."
  - Trivialement copiable si `T` l'est, et `push_back` renvoie une référence vers l'élément ajouté.
  - Un déplacement ou un `swap` coûte O(size), contre O(1) pour `std::vector`.
status: selected
draft: true
featured: false
edition: 2026-W48
---
Jusqu'à C++26, la bibliothèque standard n'avait aucun conteneur de type vecteur dont la capacité soit fixée à la compilation et dont les éléments soient stockés dans l'objet lui-même. `std::vector` passe par son allocateur, en général le tas, et `std::array` a une taille fixe. Le nouvel en-tête `<inplace_vector>` comble ce manque avec `std::inplace_vector<T, N>`, issu de P0843R14 puis affiné par P3981R2, P4022R0 et P3074R7. C'est en somme `boost::static_vector` entré dans la norme. Il sert quand on ne peut pas allouer (bare metal), quand la latence d'allocation est inacceptable, ou quand les données doivent vivre dans l'objet pour être sérialisées. L'interface reste celle d'un vecteur : `push_back`, `emplace_back`, `insert`, `erase`, `operator[]`, `at()`, `data()`… `capacity()` est `static constexpr` et vaut toujours `N`. Contrairement à `std::array`, `T` n'a pas besoin d'être constructible par défaut, car seuls les `size()` éléments présents sont construits.

L'article décrit trois façons de gérer le dépassement de capacité. `push_back` et `emplace_back` lèvent `std::bad_alloc` quand le conteneur est plein. Ce choix plutôt que `length_error` permet aux blocs `catch` qui traitent déjà les échecs d'allocation de couvrir aussi ce cas. `try_push_back` et `try_emplace_back` renvoient un `std::optional<T&>` qui vaut `nullopt` en cas d'échec, et dans ce cas l'opération n'a aucun effet : l'argument n'est pas consommé. Enfin, `unchecked_push_back` et `unchecked_emplace_back` exigent `size() < capacity()`, sinon le comportement est indéfini. Ces variantes sont surtout utiles sur les implémentations freestanding, qui ne fournissent pas les fonctions qui lèvent des exceptions.

```cpp
std::inplace_vector<std::string, 2> v;
v.push_back("first");
v.push_back("second");

std::string s = "third";
auto result = v.try_push_back(std::move(s));
if (!result) {
    // s is NOT moved-from — still holds "third"
    assert(s == "third");
}
```

Première différence avec `std::vector` : `push_back` renvoie ici une référence vers le nouvel élément. `std::vector` ne pouvait pas faire ce changement pour des raisons d'ABI, alors qu'un nouveau type n'a pas cette contrainte. Si `T` est trivialement copiable, `inplace_vector<T, N>` l'est aussi, ce qui permet de le copier octet par octet. `std::vector` ne peut jamais l'être, puisqu'il contient un pointeur vers son stockage. En revanche, un déplacement ou un `swap` coûte O(size), car chaque élément est déplacé, contre O(1) pour `std::vector`. Contrairement à un `std::pmr::vector` adossé à un `monotonic_buffer_resource` sur la pile, la limite est stricte et il n'y a pas de repli sur le tas. Le conteneur est aussi utilisable en évaluation constante en C++26, y compris avec des types non triviaux, grâce à P3074R7 et P3726R2. Pour l'auteur, il comble l'écart entre `std::array` et `std::vector`.
