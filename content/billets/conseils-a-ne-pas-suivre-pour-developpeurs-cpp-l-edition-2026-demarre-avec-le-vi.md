---
title: "Conseils (à ne pas suivre) pour développeurs C++ : l'édition 2026 démarre avec le vibe coding"
date: 2026-11-13T13:42:26.000Z
source: isocpp.org
link: https://isocpp.org//blog/2026/11/terrible-tips-for-a-cpp-developer-2026-edition-part-1
tags:
  - cpp
lang: en
level: intermediate
format: article
reading_time: 15
summary: "Andrey Karpov publie la première partie de la nouvelle édition de son livre « Terrible tips for a C++ developer ». Il y démonte quatre mauvais conseils : coder uniquement en C++, laisser l'IA générative écrire le code, ne plus lire de livres, et taper des tabulations littérales dans les chaînes."
takeaways:
  - Choisir C++ quand ses performances ou ses contraintes le justifient, pas par habitude.
  - "Du code C++ généré par l'IA peut avoir l'air optimisé tout en étant plus lent : dans le benchmark de l'article, la version de Claude Opus arrive dernière."
  - Les livres de fond restent indispensables, et les débutants qui partent directement de l'IA n'apprennent pas à déboguer.
  - Écrire `\t` plutôt qu'une tabulation littérale dans les chaînes, pour que le reformatage ne change pas le résultat.
status: published
featured: false
edition: 2026-W47
---
Andrey Karpov (PVS-Studio) commence à publier en plusieurs parties la deuxième édition de son livre, qui compte 60 « terribles conseils ». Chaque partie présente des conseils qu'il ne faut surtout pas suivre, explique pourquoi ils sont mauvais et en détaille les aspects moins évidents. La série s'adresse aux débutants, mais elle veut aussi amuser les développeurs expérimentés avec quelques bizarreries de C++. Le premier conseil, « ne coder qu'en C++ », rappelle qu'on doit choisir ce langage quand il est vraiment utile : programmation système, embarqué, IoT, environnements contraints ou très exigeants en performances. Dans ces cas, quelques pourcents d'efficacité peuvent valoir beaucoup d'argent.

Le deuxième conseil propose de ne pas apprendre C++ et de le « piloter » en vibe coding avec l'IA générative. L'auteur répond que, sans comprendre le code généré, on ne peut ni distinguer un code efficace d'un code lent, ni corriger ses erreurs, et que le régénérer en boucle aide rarement. Son exemple est une fonction `IsSpanBlank` écrite par Claude Opus avec une boucle imbriquée présentée comme favorable au SIMD. Un collègue l'a mesurée avec GCC, Clang et MSVC : une simple boucle fait mieux avec GCC et Clang en `-O3`, et la version de l'IA arrive dernière. Les variantes de DeepSeek (masque de bits, SSE2) et le remplacement de `||` par `|` donnent de bons résultats, la version SSE2 étant la plus rapide mais moins portable. Pour Karpov, juger ce genre de code demande un expert en optimisation, pas seulement quelqu'un qui connaît le langage.

Le troisième conseil prétend que les livres ne servent plus à rien et que les vidéos et l'IA suffisent. L'auteur défend les bases (conception, POO, langages), qui restent utiles. Selon lui, les vidéos poussent moins à la réflexion qu'une lecture attentive et laissent une connaissance fragmentée. Les débutants qui partent directement de l'IA n'apprennent pas à déboguer. Il cite Code Complete : aucune innovation n'a jamais réussi à supprimer la programmation. Le dernier conseil, taper une vraie tabulation dans une chaîne littérale au lieu de `\t`, est dangereux : un reformatage automatique peut remplacer les tabulations par des espaces et modifier en silence ce qu'affiche le programme, comme l'a montré un cas réel où des colonnes se sont retrouvées désalignées.
