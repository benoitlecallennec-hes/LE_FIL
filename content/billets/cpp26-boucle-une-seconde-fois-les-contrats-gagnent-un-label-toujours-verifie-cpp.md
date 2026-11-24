---
title: "C++26 bouclé une seconde fois : les contrats gagnent un label « toujours vérifié », C++29 accueille `break label;`"
date: 2026-11-21T13:39:38.000Z
source: herbsutter.com
link: https://herbsutter.com/2026/11/21/c26-is-done-again-trip-report-november-2026-iso-c-standards-meeting-buzios-brazil/
tags:
  - cpp
feed_tag: C++
lang: en
level: intermediate
format: article
reading_time: 8
summary: Le scrutin d'approbation de C++26 n'a pas été unanime, une première, à cause des contrats. À Búzios, le comité ISO C++ a rouvert puis refermé le standard en y ajoutant le label `std::contracts::controls::enforced`, et a adopté pour C++29 `break label;`, `continue label;`, `std::ranges::any_view` et `views::slice`.
takeaways:
  - Le scrutin DIS de C++26 a échoué à l'unanimité pour la première fois, à cause des contrats ; ceux-ci restent dans C++26 (92,5 % pour).
  - "Nouveau label `std::contracts::controls::enforced` : une assertion `pre`, `post` ou `contract_assert` marquée ainsi est toujours vérifiée, et l'exécution s'arrête si elle est violée."
  - C++29 adopte `break label;` et `continue label;`, les préfixes octaux `0o`, `std::ranges::any_view`, `views::slice` et `std::optional::get_ptr()`.
  - C++26 doit passer un second scrutin d'approbation avant sa publication par l'ISO.
missing:
  - image principale (og:image) non récupérée
  - liste complète des papiers adoptés pour C++29 (seuls 5 sur 12 sont nommés)
status: selected
draft: true
featured: false
edition: 2026-W48
---
Herb Sutter rend compte de la réunion ISO C++ de novembre 2026 à Búzios, au Brésil, organisée par la C++ Alliance. Elle a réuni environ 220 participants, dont 65 % à distance, et 27 pays y étaient formellement représentés. Le travail technique sur C++26 était achevé depuis mars, mais le scrutin d'approbation (DIS) n'a pas été unanime, ce qui n'était jamais arrivé. Cinq organismes nationaux ont soulevé pour la première fois des objections sur les contrats (`pre`, `post`, `contract_assert`) et ont voté contre. Le comité a consacré la plus grande partie de ses six jours à ces commentaires. Les demandes de retrait des contrats ont été débattues, remises au vote puis rejetées. Le vote final pour publier C++26 à nouveau a recueilli 111 voix pour, 9 contre et 8 abstentions, soit 92,5 % (rapport pour/contre). Le vote de mars avait donné 90,5 %, celui de février 2025 87,7 %.

La réponse principale est une extension, P4451R2 « Early Adoption of an Always-Enforced Label » de Joshua Berne, proche dans l'esprit de la proposition roumaine P3911R2. Elle permet de déclarer qu'une assertion de contrat précise est toujours vérifiée : si elle est violée, l'exécution ne continue pas, quelle que soit la sémantique des autres assertions. Le label est trouvé par la recherche de noms ordinaire, si bien qu'un alias d'espace de noms ou une directive `using` fonctionne aussi. Il est présenté comme compatible avec P3400R5, à l'étude pour C++29.

```cpp
#include <contracts>

int f(int x)
    pre<std::contracts::controls::enforced>(x > 0)
    post<std::contracts::controls::enforced>(r : r > 0)
{
    contract_assert<std::contracts::controls::enforced>(x > 0);
    return x;
}
```

Trois autres correctifs entrent dans C++26. P4466R2 rend sélectivement transparents certains déclarateurs `using`, ce qui touche la réflexion et les modules, dont le module `std`. P4453R1 donne un contrôle plus souple de l'allocation mémoire dans `std::execution`. P3861R1 étend les types structurels standard, ce qui permet notamment d'utiliser `string_view`, `span` et `tuple` comme paramètres de template. Un document final mis à jour doit maintenant passer un second scrutin DIS avant sa publication par l'ISO.

Pour C++29, le comité a adopté côté langage P3568R4, qui permet de nommer une boucle et de cibler cette boucle avec `break label;` ou `continue label;`. Il a aussi adopté P0085R5, qui introduit les préfixes octaux `0o` et `0O` et rend obsolète le simple `0`. Côté bibliothèque, il a adopté `std::ranges::any_view`, une vue à effacement de type (P3411R7), `views::slice`, qui extrait une sous-plage entre deux indices et est le premier adaptateur de plage à deux paramètres (P3216R6), et `.get_ptr()` sur `std::optional` (P4189R0). La prochaine réunion aura lieu à Istanbul en mars.
