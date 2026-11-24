---
title: "simdjson 5.0 : la réflexion statique de C++26 devient officielle et s'enrichit d'annotations de sérialisation"
date: 2026-11-16T21:49:47.000Z
source: isocpp.org
link: https://isocpp.org//blog/2026/11/simdjson-5.0-with-full-cpp26-reflection-support-daniel-lemire
tags:
  - cpp
lang: en
level: advanced
format: article
reading_time: 6
image: https://isocpp.org/files/img/simdjson5-cover-825x510.jpg
summary: Daniel Lemire annonce simdjson 5.0. La réflexion statique de C++26, que la version 4.0 gardait derrière une option, est désormais officiellement prise en charge. Elle s'accompagne d'annotations comme `rename_all` ou `skip`, qui pilotent la conversion automatique des structures C++ en JSON.
takeaways:
  - "La réflexion statique de C++26 n'est plus derrière une option : simdjson la détecte dès que le compilateur l'active (`-freflection` avec GCC 16)."
  - Des annotations (`rename`, `rename_all`, `skip`, `default_value`, `flatten`, `deny_unknown_fields`…) règlent la sérialisation automatique.
  - Par défaut, la désérialisation par réflexion lit chaque objet en une seule passe grâce à des sélecteurs de clés à hachage parfait.
  - Nouveautés sur les flux de documents (RFC 7464, `simdjson::slice_at`), les grands entiers (`BIGINT_NUMBER`) et la vitesse de sérialisation.
status: selected
draft: true
featured: false
edition: 2026-W48
---
simdjson est une bibliothèque C++ qui lit et écrit du JSON. On la trouve notamment dans Node.js, ClickHouse, Meta Velox, StarRocks, Apache Doris et le navigateur Ladybird. La version 4.0, sortie en septembre 2025, introduisait la réflexion statique de C++26 pour convertir une structure C++ en JSON, et inversement, sans code d'adaptation. Avec la 5.0, cette fonction n'est plus cachée derrière une option et devient officiellement prise en charge. Si le compilateur active la réflexion (par exemple `g++ -std=c++26 -freflection` avec GCC 16), simdjson la détecte seul et rend disponibles les fonctions qui en dépendent.

La nouvelle version ajoute des annotations pour régler la sérialisation et la désérialisation automatiques : `rename`, `rename_all`, `alias`, `skip`, `default_value`, `flatten`, `deny_unknown_fields`, `transparent`, entre autres.

```cpp
struct [[= simdjson::rename_all<simdjson::case_style::camel_case>]] User {
  std::string first_name;
  int64_t user_id;
  [[= simdjson::rename<"KEY">]] int api_key;
};
// {"firstName":"Ann","userId":7,"KEY":8}
```

Par défaut, la désérialisation par réflexion passe maintenant par des sélecteurs de clés. Ce mécanisme, disponible dès C++20, nomme les clés à la compilation et construit un hachage parfait. L'objet est alors lu en une seule passe, quel que soit l'ordre des clés. `-DSIMDJSON_DISABLE_KEY_SELECTOR_REFLECTION=1` rétablit l'ancien comportement. Un entier positif compris entre 2^64 et 10^20 est désormais signalé comme grand entier (`BIGINT_NUMBER`) au lieu d'être rejeté comme nombre mal formé. Côté flux de documents, la 5.0 accepte les séquences JSON de la RFC 7464, les documents séparés par des virgules et `stream_format::newline_delimited`. La fonction `simdjson::slice_at` découpe un flux aux limites des documents pour un traitement sur plusieurs threads.

Parmi les autres ajouts : des accesseurs étroits avec contrôle de plage (`get_uint8()`, `get_int16()`…), `std::float32_t` et `std::float64_t` en C++23, l'analyse sans remplissage avec `parse_unpadded`, la compatibilité avec les vues de `std::ranges`, l'affichage indenté dans le style FracturedJson, les fichiers projetés en mémoire sous Windows et la prise en charge du compilateur Fil-C. L'affichage des flottants passe de Grisu2 à Dragonbox. Selon les mesures de l'auteur, la sérialisation de certains fichiers gagne nettement en vitesse, par exemple de 0,31 à 0,52 Go/s sur canada.
