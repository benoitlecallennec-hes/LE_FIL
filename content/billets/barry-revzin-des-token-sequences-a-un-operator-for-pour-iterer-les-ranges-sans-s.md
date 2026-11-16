---
title: "Barry Revzin : des token sequences à un `operator for` pour itérer les ranges sans surcoût"
date: 2026-11-04T06:00:00.000Z
source: Barry Revzin
link: https://brevzin.github.io/c++/2026/11/04/operator-for/
tags:
  - cpp
feed_tag: c++
lang: en
level: advanced
format: article
reading_time: 15
summary: Barry Revzin reprend son expérience d'itération de ranges par injection de tokens, la porte sur la nouvelle proposition P4380 implémentée dans Clang, puis en conclut qu'un vrai mécanisme du langage, `operator for`, serait plus propre.
takeaways:
  - "`views::take_while` et `views::reverse` coûtent cher avec la boucle `for` classique ; Flux règle le problème par itération interne."
  - Avec P4380 et Clang, une macro `for_each!` réécrit la boucle selon chaque adaptateur, mais il faut traduire `break` en `goto` et le modèle n'a aucune hygiène des noms.
  - "`operator for` réécrit une boucle en une autre boucle avec `co_yield`, ce qui garde `return` et les coroutines utilisables, contrairement aux callables de P2881."
  - Le parcours en sens inverse demande une surcharge `backward_t<T>` ; Revzin admet que ce point duplique du code.
status: rejected
draft: true
featured: false
---
Il y a un an et demi, Barry Revzin montrait que certains adaptateurs de ranges, en particulier `views::take_while` et `views::reverse`, ont un surcoût inévitable parce qu'ils s'intègrent mal à la boucle `for` classique du C++. La bibliothèque Flux de Tristan Brindle contourne ce problème par itération interne, avec un `for_each` propre. Revzin avait imité cette approche avec l'injection de tokens de l'implémentation EDG de Daveed Vandevoorde. Il la porte maintenant sur la nouvelle proposition P4380 et son implémentation Clang. Le résultat est une macro `for_each!(int& i : v | std::views::filter(is_even) | std::views::reverse, { ... })`. Elle découpe l'en-tête au `:` de premier niveau, puis choisit entre la boucle personnalisée fournie par le range, une boucle avant ou une boucle arrière.

Chaque adaptateur se personnalise par un `loop_traits<R>`, trouvé avec `std::meta::lookup`. Il fournit une fonction `loop` qui génère du code et marque chaque élément produit par un faux `co_yield`. `filter` génère un `if`. `join` imbrique deux boucles. `reverse` inverse la direction. `take_while` sort de la boucle quand le prédicat échoue en marche avant ; en marche arrière, il calcule d'abord la sous-plage valide. Chaque `co_yield` est remplacé par une affectation de l'élément suivie du corps de l'utilisateur, enveloppé dans `do { ... } while (false);` pour que `continue` fonctionne. `break` est plus délicat : dans la boucle interne d'un `join`, il doit aussi sortir de la boucle externe. Revzin le réécrit donc en `goto` vers une étiquette placée après la boucle, ce qui l'oblige à analyser le corps. Le modèle n'a aucune hygiène des noms : il faut recourir à `std::meta::unique_id()` pour éviter les collisions.

Revzin compare ce résultat à P2881, sa proposition de 2023 écrite avec Jonathan Müller. P2881 transformait le corps de la boucle en callable, ce qui empêchait d'utiliser `return` ou les opérations de coroutine. `operator for` réécrit au contraire une boucle en une autre boucle, comme les token sequences. Un adaptateur l'adopte en déclarant un `operator for()` dont le corps utilise `co_yield` :

```cpp
operator for() {
    for (auto&& e : base_) {
        if (std::invoke(*pred_, e)) {
            co_yield e;
        }
    }
}
```

Le parcours en sens inverse demande une surcharge distincte qui prend un `backward_t<T>`, par exemple `friend operator for(backward_t<filter_view> b)`. Revzin reconnaît que cela duplique du code et demande de meilleures idées. Tristan Brindle a déjà écrit rapidement plusieurs adaptateurs Flux avec cette approche, et le code optimisé obtenu est bon. Selon Revzin, l'injection de tokens fonctionne mais reste trop bricolée pour une bibliothèque réelle. Un mécanisme de premier ordre dans le langage serait plus propre, même si une injection de tokens puissante resterait utile pour expérimenter.
