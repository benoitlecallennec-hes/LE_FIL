---
title: Les six façons de terminer un programme C++, et ce que chacune nettoie
date: 2026-11-17T23:00:00.000Z
source: sandordargo.com
link: https://www.sandordargo.com/blog/2026/11/18/ways-to-end-a-cpp-program
tags:
  - cpp
feed_tag: dev
lang: en
level: intermediate
format: article
reading_time: 8
summary: "Sandor Dargo passe en revue les façons de terminer un programme C++ : `return` depuis `main`, `std::exit`, `std::quick_exit`, `std::abort`, `std::terminate` et `std::_Exit`. Pour chacune, il précise quels destructeurs et quels gestionnaires sont appelés, et si les flux sont vidés."
takeaways:
  - "`std::exit` appelé au fond d'une fonction exécute le nettoyage des statiques et des flux, mais ne détruit pas les variables locales des fonctions appelantes."
  - "`std::quick_exit` n'exécute que les gestionnaires `std::at_quick_exit`, ce qui évite les blocages pendant la destruction des statiques."
  - "`std::abort` garde l'état du programme pour l'analyse après un plantage, alors que `std::_Exit` renvoie un code de sortie normal sans rien nettoyer."
  - Par défaut, `std::terminate` appelle `std::abort`, par exemple quand une exception sort d'une fonction `noexcept`.
missing:
  - image principale de la page (og:image)
status: published
featured: false
edition: 2026-W48
---
L'article part d'une énumération lue dans le livre *C++ Memory Management* de Patrice Roy. Le cas normal est le `return` depuis `main` : les objets locaux de `main` sont détruits, puis la valeur renvoyée est utilisée comme si on appelait `std::exit` avec elle. Sans `return`, tout se passe comme avec `return 0`. Sous POSIX, le code de sortie tient sur 8 bits, donc `256` devient `0`. L'auteur conseille de ne pas dépasser `125` pour ses propres codes d'échec et rappelle que `EXIT_SUCCESS` et `EXIT_FAILURE` sont les constantes portables.

`std::exit` détruit les objets `thread_local` du thread courant, mais pas ceux des autres threads. Il détruit aussi les objets statiques, en les entremêlant avec les gestionnaires `atexit`, dans l'ordre inverse de leur construction ou de leur enregistrement. Il vide et ferme les flux `FILE*` puis supprime les fichiers créés par `tmpfile()`. Les variables locales des fonctions appelantes, elles, ne sont pas détruites. Appeler `std::exit` depuis un destructeur statique ou un gestionnaire `atexit` est un comportement indéfini. Selon l'auteur, une bibliothèque ne devrait jamais appeler `exit`, sauf si elle possède explicitement `main`.

```cpp
[[noreturn]] void fatal_error(const char* msg) {
    std::cerr << "fatal: " << msg << "\n";
    std::exit(1);
}
```

`std::quick_exit` (C++11) existe parce que la destruction des statiques peut provoquer un interblocage ou un plantage, par exemple à cause du fiasco de l'ordre de destruction, ou être lente quand il faut fermer beaucoup de fichiers. Il exécute les gestionnaires `std::at_quick_exit` dans l'ordre inverse de leur enregistrement, puis appelle `std::_Exit`, sans détruire aucun objet ni vider les flux. `std::abort` lève `SIGABRT` et sert aux assertions échouées et aux états impossibles. Il n'exécute ni destructeurs ni gestionnaires de sortie, mais produit en général un core dump et s'arrête dans le débogueur s'il y en a un d'attaché.

`std::terminate` est le dernier recours du runtime et appelle `std::abort` par défaut. Le runtime l'appelle quand une exception n'est pas rattrapée, quand elle sort d'une fonction `noexcept` ou d'un destructeur pendant le déroulement de la pile, et quand elle s'échappe de la construction d'un objet statique ou d'un gestionnaire `atexit`. On peut le remplacer avec `std::set_terminate`, mais le gestionnaire installé ne doit pas rendre la main. Enfin, `std::_Exit` ne fait aucun nettoyage : pas de vidage des flux, pas de suppression des fichiers temporaires. Contrairement à `abort`, il renvoie au processus parent un code de sortie normal, qui ne ressemble donc pas à un plantage.
