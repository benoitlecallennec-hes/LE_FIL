---
title: "Constantes nommées en C : quand une constante cache aussi un objet"
date: 2026-11-06T16:02:44.000Z
source: Jens Gustedt
link: https://gustedt.wordpress.com/2026/11/06/named-constants-and-supporting-objects-in-c/
tags:
  - c
feed_tag: C23
lang: en
level: advanced
format: article
reading_time: 3
summary: Jens Gustedt explique pourquoi, en C, certaines constantes s'appuient aussi sur un objet en mémoire. Il montre ce que C23 et la future révision C2y changent pour les déclarations `const` et `constexpr`.
takeaways:
  - "En C11, `size_t const N = 33;` n'est pas une expression constante entière : `double a[N]` devient un VLA."
  - Avec N3833, acceptée pour C2y, ces déclarations `const` entières deviennent implicitement `constexpr`.
  - "Une constante nommée déclarée par `const` ou `constexpr` reste un objet dont on peut prendre l'adresse ; `enum : size_t { … }` donne une constante sans objet."
  - "`constexpr` permet aussi des constantes de types non entiers, y compris des tableaux, qui ont besoin d'un objet sous-jacent."
status: published
featured: false
edition: 2026-W47
---
En C traditionnel, une déclaration comme `size_t const MyBound0 = 33;` donne une valeur fixe, mais ce n'est pas une « expression constante entière » au sens de la norme. En C11, un tableau `double array[MyBound0] = {};` était donc un tableau de longueur variable (VLA), dont la taille n'est connue qu'à l'exécution. Pire, son initialisation par `{}` était invalide, puisqu'elle était interdite pour les VLA. C23 corrige une partie du problème en autorisant l'initialisation par défaut des VLA, mais la situation reste peu satisfaisante pour les développeurs.

```c
size_t const MyBound0 = 33;
...
double array[MyBound0] = {};
```

Le comité WG14 a accepté la proposition N3833 de JeanHeyd Meneide. Dans C2y, la prochaine révision de C, une déclaration `const` de type entier initialisée par une expression constante entière devient implicitement `constexpr`. `MyBound0` sera alors une vraie expression constante entière, et `array` un tableau ordinaire. L'identifiant garde toutefois un double rôle : c'est une constante partout où il est évalué, mais aussi un objet dont on peut prendre l'adresse. Il faut le conserver pour la compatibilité, car beaucoup de code existant utilise cette syntaxe. WG14 a introduit le terme de « constante nommée » (named constant) pour désigner ces identifiants et tous ceux qui sont remplacés par une valeur à la traduction.

C23 propose aussi `constexpr` (`constexpr size_t MyBound1 = 33;`) comme manière moderne de déclarer une expression constante entière. Il souligne le caractère constant tout en gardant l'objet en coulisse : stockage statique au niveau global, automatique dans une fonction. Pour obtenir une constante pure, sans objet sous-jacent, on peut écrire `enum : size_t { MyBound2 = 33 };`, avec le type sous-jacent fixe introduit par C23. L'auteur note avec ironie que des collègues risqueraient d'y voir une simple coquetterie.

Enfin, `constexpr` comble un autre manque du langage : il permet des expressions constantes de types autres qu'entiers, y compris des tableaux. Comme les tableaux se convertissent très vite en pointeurs en C, il importe de pouvoir raisonner sur un objet sous-jacent dans ce cas aussi.
