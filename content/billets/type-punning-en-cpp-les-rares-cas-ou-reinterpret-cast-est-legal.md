---
title: "Type punning en C++ : les rares cas où `reinterpret_cast` est légal"
date: 2026-11-12T19:01:55.000Z
source: isocpp.org
link: https://isocpp.org//blog/2026/11/type-punning-done-right-mateusz-pusz
tags:
  - cpp
lang: en
level: advanced
format: article
reading_time: 10
summary: Dans le quatrième volet de sa série sur les fondamentaux du C++, Mateusz Pusz dresse la liste fermée des usages de `reinterpret_cast` définis par la norme et indique comment convertir proprement une valeur d'un type à un autre.
takeaways:
  - "`reinterpret_cast` ne convertit pas une valeur : pour cela, utiliser `std::bit_cast` ou `std::memcpy`"
  - Lire les octets d'un objet via `char`, `unsigned char` ou `std::byte` est légal, mais pas les octets de padding ni l'écriture dans un objet vivant
  - "Sont aussi légaux : l'accès via la variante signée ou non signée d'un type, le passage d'une `struct` standard-layout à son premier membre et l'aller-retour d'un pointeur par `std::uintptr_t`"
  - En dehors de cette liste fermée, c'est un comportement indéfini
status: published
featured: false
edition: 2026-W47
---
Mateusz Pusz part d'un principe posé dans les volets précédents : `reinterpret_cast` change seulement l'étiquette d'un pointeur ou d'une référence. Il ne transforme jamais les octets d'un type en valeur d'un autre type. Lire les bits d'un `double` à travers un `std::int64_t*` est donc un comportement indéfini, puisqu'aucun objet `int64_t` n'existe à cette adresse. Pour réinterpréter une valeur, il faut copier ses octets avec `std::bit_cast`, ou avec `std::memcpy` quand `bit_cast` n'est pas disponible.

Premier cas autorisé : lire la représentation mémoire de n'importe quel objet via `char`, `unsigned char` ou `std::byte`, par exemple avec `std::as_bytes`. C'est ce que font la sérialisation, le hachage et les dumps hexadécimaux. L'auteur précise les limites de ce cas. Seuls les types trivialement copiables ou standard-layout ont des octets contigus garantis. Lire les octets de padding est indéfini, et `std::has_unique_object_representations_v` permet d'exclure leur présence sans pouvoir la prouver. Écrire dans un objet vivant via `std::as_writable_bytes` est indéfini. Les vues en octets nuisent aussi à l'optimisation, parce que ces types peuvent faire alias avec tout. Enfin, le texte de la norme est encore incomplet sur ce point, et la proposition P1839 doit le corriger.

Deux autres cas sont légaux. On peut accéder à un objet via la variante signée ou non signée de son propre type : c'est le seul cas où l'on obtient « mêmes bits, autre type » sans copie. Pour une classe standard-layout, la structure et son premier membre non statique sont « pointer-interconvertibles », ce que l'on peut vérifier avec `std::is_pointer_interconvertible_with_class`. Cette garantie ne vaut que pour le premier membre : revenir à la structure depuis un autre membre avec `offsetof` reste indéfini, même si P3407 propose de changer cela.

Dernier cas : un pointeur converti en `std::uintptr_t` puis reconverti redonne un pointeur utilisable, ce qui sert aux pointeurs étiquetés, au log d'adresses ou au hachage par identité. L'arithmétique sur l'entier n'est valable que si le résultat correspond à une adresse qu'un pointeur converti aurait pu produire, comme le clarifie P2434, adoptée en juin 2026 comme rapport de défaut. Pour parcourir un buffer, il faut faire l'arithmétique sur un `std::byte*`. L'auteur ajoute à la liste l'accès via `std::launder` à un objet à durée de vie implicite créé par `std::memcpy`. Il recommande d'éviter `reinterpret_cast` et les pointeurs bruts autant que possible, car ces casts contournent les informations de type sur lesquelles s'appuie l'optimiseur.
