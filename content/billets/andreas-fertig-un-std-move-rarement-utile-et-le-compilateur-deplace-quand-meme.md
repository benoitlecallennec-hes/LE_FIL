---
title: "Andreas Fertig : un `std::move` rarement utile, et le compilateur déplace quand même"
date: 2026-11-23T21:46:23.000Z
source: isocpp.org
link: https://isocpp.org//blog/2026/11/move-in-cpp-without-a-stdmove-andreas-fertig
tags:
  - cpp
lang: en
level: intermediate
format: article
reading_time: 2
image: https://isocpp.org/files/img/andreas_fertig.png
summary: "Andreas Fertig a dit qu'il fallait se servir de `std::move` le moins possible. Il montre ici ce que ce conseil rapporte : des performances optimales par défaut, grâce à l'élision de copie et aux déplacements implicites, qui s'étendent encore en C++23."
takeaways:
  - Le RVO pur (`return {};`) bénéficie d'une élision de copie garantie depuis C++17 ; le NRVO n'est pas garanti, mais il ne coûte en général rien.
  - Un paramètre passé par valeur puis renvoyé est déplacé implicitement depuis C++11.
  - Avant C++20, GCC copie un paramètre `Apple&&` renvoyé par valeur, alors que Clang le déplace.
  - En C++23, renvoyer un paramètre `Apple&&`, par valeur ou en `Apple&&`, déclenche un déplacement implicite sans `std::move`.
missing:
  - image principale (og:image) de l'article original
status: selected
draft: true
featured: false
edition: 2026-W48
---
Selon Andreas Fertig, les copies inutiles comptent parmi les pires ennemis des performances. Il faut donc viser d'abord l'optimisation de la valeur de retour (RVO). L'objet renvoyé n'est alors pas créé sur la pile de la fonction : il est construit directement chez l'appelant, là où il finira de toute façon. Cela évite à la fois la copie et le déplacement. La norme, qui ne parle jamais d'optimisations du compilateur, appelle ce mécanisme élision de copie (*copy elision*). Depuis C++17, elle est garantie pour le RVO pur. La variante nommée (NRVO) ne l'est pas, mais en pratique on n'y paie probablement ni copie ni déplacement.

```cpp
Apple RVO()
{
  return {};
}

Apple NRVO()
{
  Apple res{};

  return res;
}
```

Quand l'élision n'est pas possible, la meilleure solution suivante est le déplacement. Depuis C++11, un paramètre passé par valeur puis renvoyé est déplacé implicitement (`Apple Fun(Apple val)`). Deux cas sont plus délicats. Le premier renvoie par valeur un paramètre reçu en référence de rvalue (`Apple Cat(Apple&& val)`) : avant C++20, GCC copie la valeur de retour alors que Clang la déplace. L'auteur en conclut que ne pas suivre la norme à la lettre est parfois préférable. Le second renvoie lui-même une référence de rvalue (`Apple&& Dog(Apple&& val)`) : sans `std::move` explicite sur la valeur de retour, le code ne compile pas.

Dans ces deux cas, il faut déplacer la valeur de retour pour obtenir le meilleur résultat, ce qui va à l'encontre du conseil initial. En mode C++23, les deux cas font un déplacement implicite sans `std::move`. La règle d'Andreas Fertig, se servir de `std::move` rarement, s'applique donc encore plus.
