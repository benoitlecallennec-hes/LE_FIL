---
title: Des arguments nommés en C grâce aux initialiseurs désignés
date: 2026-11-16T00:01:00.000Z
source: Arthur O'Dwyer
link: https://quuxplusone.github.io/blog/2026/11/16/named-arguments-in-c/
tags:
  - c
feed_tag: abi
lang: en
level: intermediate
format: article
reading_time: 4
summary: Arthur O'Dwyer montre comment une macro variadique et un littéral composé, avec les initialiseurs désignés complets de C, donnent aux fonctions des arguments nommés qui ont des valeurs par défaut, comme dans `ideal_volume(.kelvin = 373.15, .moles = 2)`.
takeaways:
  - Une macro variadique qui construit un littéral composé `(struct X){ défauts, __VA_ARGS__ }` donne à une fonction C des arguments nommés et optionnels.
  - "L'idiome dépend de deux libertés propres à C : des désignateurs dans n'importe quel ordre, et des désignateurs répétés dont le dernier l'emporte (Clang avertit avec `-Winitializer-overrides`)."
  - "Ajouter un champ ne casse pas l'API, mais casse l'ABI : il faut recompiler les appelants."
  - L'auteur réserve ce motif à des cas particuliers. raddebugger d'Epic Games l'utilise dans `ui_anim`.
status: selected
draft: true
featured: false
edition: 2026-W48
---
Dans un billet précédent, Arthur O'Dwyer présentait la version « bridée » des initialiseurs désignés de C++20. Il montre ici ce que permettent ceux de C, sans restriction. Il a découvert l'idiome dans le livre de Ben Klemens *21st Century C* (2014). Les paramètres sont regroupés dans une structure. La fonction réelle, `ideal_volume_impl`, la reçoit par valeur. Une macro du même nom que la fonction publique construit un littéral composé qui fixe d'abord les valeurs par défaut, puis ajoute `__VA_ARGS__`.

```c
struct IdealVolumeArgs {
    double atm;    // P
    double moles;  // n
    double kelvin; // T
};
#define ideal_volume(...) ideal_volume_impl( \
    (struct IdealVolumeArgs){ \
        .atm=1, .moles=1, .kelvin=273.15, __VA_ARGS__ \
    })

ideal_volume(.kelvin = 373.15, .moles = 2)
```

L'astuce repose sur deux propriétés de C que C++20 n'a pas reprises. Les désignateurs peuvent apparaître dans n'importe quel ordre, et un même désignateur peut être répété : seul le dernier compte. Ce second point serait une faute de style s'il n'était pas voulu. Clang le signale par défaut avec `-Winitializer-overrides`, alors que GCC n'émet `-Woverride-init` qu'avec `-Wextra`, un choix que l'auteur juge plus sensé.

Une fois cet idiome en place, ajouter un paramètre nommé ne casse plus l'API. Dans l'exemple de l'auteur, on ajoute un champ `R` pour la constante des gaz, et les anciens appels compilent toujours avec le même comportement. L'ABI, elle, change : les appelants n'ont pas à être réécrits, mais doivent être recompilés avec le nouvel en-tête. Plusieurs fonctions peuvent partager la même structure (`IdealGasArgs`) : chaque macro met à `NAN` la grandeur à calculer. On peut aussi faire précéder les arguments nommés d'arguments positionnels, comme dans `sc_draw_circle(sc, ...)`.

L'auteur déconseille pourtant de généraliser ce motif, qu'il trouve lourd et « mignon » à côté d'une fonction ordinaire, laquelle suffit presque toujours. Une recherche sur GitHub lui a permis d'en trouver quelques usages réels. Le plus en vue est le débogueur raddebugger d'Epic Games, où la macro `ui_anim` prend des paramètres d'animation nommés comme `.initial` ou `.rate`.
