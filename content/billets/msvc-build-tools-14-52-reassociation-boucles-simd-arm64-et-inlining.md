---
title: "MSVC Build Tools 14.52 : réassociation, boucles, SIMD ARM64 et inlining"
date: 2026-11-17T18:23:46.000Z
source: Microsoft C++ Team Blog
link: https://devblogs.microsoft.com/cppblog/c-optimizations-in-msvc-build-tools-14-52/
tags:
  - cpp
feed_tag: C++
lang: en
level: advanced
format: article
reading_time: 15
summary: "L'équipe C++ de Microsoft présente les nouvelles optimisations de MSVC Build Tools 14.52, livré avec Visual Studio 2026. Au programme : réassociation arithmétique, transformations de boucles, vectorisation SLP, motifs NEON sur ARM64, inlining et reconnaissance de motifs sous `/O2` et `/fp:fast`."
takeaways:
  - La réassociation repère maintenant les sous-expressions communes et les invariants de boucle ; pour les flottants, il faut `/fp:fast`.
  - Calcul direct de la valeur finale des variables d'induction, et modes d'adressage choisis pour toute la boucle (`LDP` sur ARM64).
  - La vectorisation SLP gère les nœuds phi et les accès contigus, et de nombreux motifs NEON se simplifient sur ARM64.
  - Une fonction est jugée petite pour l'inlining jusqu'à 50 instructions internes (40 auparavant), avec un bonus quand l'inlining débloque SROA ; une division répétée devient une multiplication par l'inverse sous `/fp:fast`.
missing:
  - image principale de la page
status: rejected
draft: true
featured: false
---
Troy Johnson passe en revue les optimisations ajoutées au compilateur MSVC 14.52, plus quelques-unes arrivées dans la 14.51 sans avoir été présentées. La réassociation arithmétique cherche désormais les sous-expressions communes, alors qu'elle se contentait jusqu'ici de simplifier puis d'équilibrer l'arbre d'expression. Le compilateur peut réassocier librement les entiers, mais les flottants seulement sous `/fp:fast`. Il range les termes dans l'ordre suivant : littéraux, invariants de boucle, puis variants. Il tient aussi compte de la fréquence globale de chaque terme, même quand il apparaît avec le signe opposé. L'algorithme s'inspire de Briggs et Cooper (« Effective Partial Redundancy Elimination »). Ainsi, `1.0 - d` n'est plus calculé qu'une fois, et dans la boucle ci-dessous, `1 + x` sort de la boucle tandis que `2 * i` est partagé :

```cpp
void foo2(int n, int x, int* out, int* out2) {
  for (int i = 0; i < n; ++i) {
    out[i] = 1 + 2 * i + x;
    out2[i] = 1 - 2 * i + x;
  }
}
```

Côté boucles, le compilateur peut remplacer les mises à jour répétées d'une variable d'induction par sa valeur finale. Une boucle qui ne fait qu'ajouter 5 dix fois devient `result = 50;`. Avec un nombre d'itérations connu seulement à l'exécution, elle devient `result = n > 0 ? n * 5 : 0;`. Les modes d'adressage sont maintenant choisis en regardant toute la boucle et non plus instruction par instruction. Sur ARM64, des chargements voisins peuvent ainsi fusionner en `LDP`, et une base commune `basePtr + memberOffset` est calculée une seule fois, ce qui fait passer une séquence de 16 à 9 instructions. Enfin, une opération pure invariante peut sortir de la boucle même si elle se trouve dans un bloc conditionnel.

La vectorisation SLP sait maintenant traiter les nœuds phi : deux accumulateurs deviennent deux voies d'un même vecteur. Elle reconnaît aussi que `x[j]` et `x[j + 1]` sont contigus. Sur ARM64, les intrinsèques NEON de chargement et de stockage sont ramenées à des accès mémoire génériques, que les optimisations existantes peuvent alors traiter. De nombreux motifs NEON se simplifient : multiplication par une voie choisie (`fmul v0.4s, v0.4s, v1.s[1]`), masques rendus inutiles par un décalage, rotations par voie avec `usra` ou `rev32`, suppression du travail fait dans des voies inutilisées, inversion de bits en `rbit`. La 14.51 avait déjà apporté la vectorisation de petites boucles, `uadalp`/`sadalp` et la reconnaissance des réductions scalaires.

L'inliner remonte de 40 à 50 instructions internes la taille en dessous de laquelle une fonction compte comme petite. Il réactive l'heuristique de site d'appel unique à l'intérieur des fonctions `__forceinline`. Il favorise aussi les appels dont l'inlining permet le remplacement scalaire des agrégats (SROA) et l'élimination des écritures mortes. Pour finir, de nouveaux motifs sont reconnus sur x64 : une seule division pour un diviseur répété, remplacée ensuite par des multiplications par l'inverse (sous `/fp:fast`), des `cmov` avec constantes non nulles, `(a | b) & ~(a & b)` réduit à un `xor`, et l'équivalence de comparaisons inversées ou de min/max écrits différemment.
