---
title: "`std::string` vu de l'intérieur : la petite optimisation des chaînes courtes"
date: 2026-11-09T15:18:10.000Z
source: meetingcpp.com
link: https://www.meetingcpp.com/blog/items/A-short-visit-to-std--string.html
tags:
  - cpp
lang: en
level: intermediate
format: article
reading_time: 3
summary: Jens Weller montre comment `std::string` évite d'allouer sur le tas pour les chaînes courtes, en partant d'une version simplifiée construite avec `std::variant` puis avec une `union`.
takeaways:
  - En général, `std::string` fait 32 octets et range les chaînes courtes dans l'objet lui-même, sans allocation sur le tas (SSO).
  - En mode long, il stocke `size`, `capacity` et un pointeur `char*`, soit 24 octets ; en mode court, il utilise un tampon de caractères dont un octet sert au zéro final.
  - Une `union` est 8 octets plus petite qu'un `std::variant`, qui doit savoir quel membre est actif.
  - Pour loger l'allocateur sans coût, on utilise une paire compressée (EBO) ou, depuis C++20, `[[no_unique_address]]`.
status: published
featured: false
edition: 2026-W47
---
Ce billet est le cinquième d'une série de Meeting C++ consacrée à la bibliothèque standard ; le précédent portait sur les conteneurs et les vues. Jens Weller y explique qu'un `std::string` occupe en général 32 octets et fonctionne selon deux modes. En mode « chaîne longue », il contient une taille, une capacité et un pointeur vers une zone allouée sur le tas. En mode « chaîne courte », il range les caractères directement dans ses propres octets, sans aucune allocation. Le détail varie selon les implémentations de la bibliothèque standard.

Pour le montrer, l'auteur écrit une version simplifiée. La partie longue est une structure de 24 octets. Associée à un `std::array<char,24>` dans un `std::variant`, elle donne un objet de 32 octets, soit la taille de `std::string`. Le tableau court peut recevoir 23 caractères, puisque la chaîne doit se terminer par un zéro.

```cpp
struct long_str
{
 size_t size=0,capacity=0;
 char* data;
};

union long_short{long_str long_string;std::array<char,24> short_string;};
```

Remplacer le `std::variant` par une `union` fait gagner 8 octets, ceux que le variant consomme pour savoir quel membre est actif. Le tableau court pourrait donc grandir de 8 octets sans dépasser la taille de `std::string`. Les vraies implémentations regroupent aussi ce stockage avec l'allocateur grâce à une « paire compressée », qui repose sur l'optimisation des classes de base vides (EBO). Depuis C++20, l'attribut `[[no_unique_address]]` permet d'obtenir le même résultat. Pour aller plus loin, l'article renvoie à une analyse détaillée de la chaîne de libc++, l'implémentation de clang.
