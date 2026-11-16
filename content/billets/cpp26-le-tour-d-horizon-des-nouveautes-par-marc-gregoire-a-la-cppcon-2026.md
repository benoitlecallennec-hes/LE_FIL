---
title: "C++26 : le tour d'horizon des nouveautés par Marc Gregoire à la CppCon 2026"
date: 2026-11-11T15:00:06.000Z
source: CppCon
link: https://www.youtube.com/watch?v=ak1zrDo2y-U
tags:
  - cpp
lang: en
level: intermediate
format: video
image: https://i2.ytimg.com/vi/ak1zrDo2y-U/hqdefault.jpg
summary: À la CppCon 2026, Marc Gregoire passe en revue en une heure les changements les plus marquants de C++26, de la réflexion et des contrats jusqu'aux petites améliorations comme `std::println()` sans argument.
takeaways:
  - Les deux nouveautés majeures du langage en C++26 sont la réflexion et les contrats.
  - "`= delete(\"reason\")` permet d'expliquer pourquoi une fonction est supprimée, et `#embed` intègre des données binaires dès le préprocesseur."
  - La bibliothèque gagne notamment `std::inplace_vector`, `std::philox`, `std::text_encoding` et l'arithmétique saturée.
  - La session donne une vue d'ensemble et renvoie vers d'autres conférences CppCon pour les détails.
status: published
featured: false
edition: 2026-W47
---
Comme pour les versions précédentes de la norme, Marc Gregoire, auteur de « Professional C++ » et fondateur du Belgian C++ Users Group, présente à la CppCon 2026 une sélection des nouveautés de C++26. Il ne détaille pas chacune des centaines de propositions adoptées par le comité ISO : il choisit les changements qui comptent le plus, des évolutions majeures aux petites améliorations du quotidien.

Côté langage, la session aborde la réflexion, les contrats, les variables de substitution sans nom, la suppression motivée avec `= delete("reason");`, l'indexation de packs, la directive `#embed`, les exceptions et le placement `new` en `constexpr`, ainsi que les déclarations `friend` variadiques.

Côté bibliothèque standard, la session couvre la bibliothèque de contrôle d'exécution, plusieurs nouveaux en-têtes, `std::inplace_vector` (un vecteur redimensionnable à capacité fixe), le moteur aléatoire à compteur `std::philox`, l'identification d'encodages de texte avec `std::text_encoding`, davantage de `constexpr` pour les conteneurs et leurs adaptateurs, l'arithmétique saturée, de nouveaux préfixes SI et l'affichage de lignes vides avec `std::println()`.

Pour les sujets les plus riches, l'orateur renvoie vers d'autres sessions de la CppCon qui les traitent en détail. Les diapositives sont publiées dans le dépôt GitHub CppCon2026.
