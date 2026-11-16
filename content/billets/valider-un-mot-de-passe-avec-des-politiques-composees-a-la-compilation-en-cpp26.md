---
title: Valider un mot de passe avec des politiques composées à la compilation en C++26
date: 2026-11-07T00:58:32.000Z
source: isocpp.org
link: https://isocpp.org//blog/2026/11/compile-time-policy-composition-for-password-validation-daniel-gomez-vergel
tags:
  - cpp
lang: en
level: advanced
format: article
reading_time: 6
summary: Daniel Gómez Vergel construit un validateur de mots de passe configuré entièrement à la compilation. Chaque règle est un type, et on les combine avec `operator|` au lieu d'utiliser le polymorphisme dynamique et le pattern Decorator.
takeaways:
  - "Chaque règle de validation est un type : la configuration du validateur est encodée dans son type, sans appel virtuel ni allocation."
  - Le concept `Character_policy` impose un objet fonction constructible par défaut, appelable avec un `char`, `noexcept` et dont le résultat est convertible en `bool`.
  - Les instructions d'expansion `template for` de C++26 et l'indexation de pack `Policies...[Idx]` servent à parcourir les politiques, et un `std::bitset` permet de s'arrêter tôt.
  - "`operator|` ajoute les politiques une à une, et un `static_assert` empêche les doublons."
status: published
featured: false
edition: 2026-W47
---
Une solution orientée objet classique combinerait les règles de validation à l'exécution, avec du polymorphisme dynamique et le pattern Decorator. Daniel Gómez Vergel choisit une autre voie : toute la composition se fait à la compilation, et la configuration du validateur fait partie de son type. Il n'y a donc ni appel virtuel ni allocation dynamique. L'auteur présente sa bibliothèque comme un exemple pédagogique et non comme un outil de production. Elle a été testée avec GCC 16.2.

Les règles de base sont une longueur comprise entre deux bornes et l'absence d'espace ASCII. Quatre politiques au niveau du caractère s'y ajoutent : présence d'un chiffre, d'une minuscule, d'une majuscule et d'un caractère spécial. Chacune est un objet fonction sans état, accompagné d'une instance constante utilisable dans un pipeline. Le concept `Character_policy` encadre ces types : ils doivent être constructibles par défaut et appelables avec un `char`, et l'appel doit être `noexcept` et renvoyer une valeur convertible en `bool`.

Le cœur est un template de classe variadique `Password_validation`. Il reçoit les bornes de longueur et un pack de politiques, puis valide en trois étapes : la longueur, les espaces, puis le respect des politiques. Un `std::bitset` garde une trace des politiques satisfaites, à raison d'un bit par politique, et le parcours s'arrête dès que tous les bits sont à 1. Le pack est parcouru avec les instructions d'expansion `template for` de C++26, sur `std::views::indices(policy_count)`, et `Policies...[Idx]` sélectionne chaque type.

Pour la composition, une surcharge de `operator|` renvoie un nouveau type de validateur qui contient une politique de plus, par exemple `check_0 | digit | lowercase`. Un `static_assert` refuse l'ajout d'une politique déjà présente. L'exemple final, dans `main()`, importe le module et construit un validateur pour des mots de passe de 8 à 16 caractères exigeant les quatre politiques. Il vérifie le type obtenu avec un `static_assert`, puis lit un mot de passe sur `std::cin` et affiche s'il est valide.
