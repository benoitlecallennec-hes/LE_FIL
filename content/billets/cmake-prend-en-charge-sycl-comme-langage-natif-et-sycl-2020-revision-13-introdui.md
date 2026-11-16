---
title: CMake prend en charge SYCL comme langage natif, et SYCL 2020 Revision 13 introduit des en-têtes séparés
date: 2026-11-12T18:11:00.000Z
source: khronos.org
link: https://www.khronos.org/blog/sycl-revision-13
tags:
  - 3d
lang: en
level: intermediate
format: article
reading_time: 7
image: https://www.khronos.org/images/jcogs_img/cache/cmake_speaks_sycl_-_28de80_-_ade52cc9e2371411e2f6a5e18fb8664a9767175d.png
summary: CMake 4.5 apporte une prise en charge native de SYCL, expérimentale et à activer explicitement, pour AdaptiveCpp, les deux distributions de DPC++ et Clang. En parallèle, SYCL 2020 Revision 13 propose des en-têtes séparés qui peuvent réduire les temps de compilation.
takeaways:
  - "CMake 4.5 : SYCL est un langage natif, en mode expérimental et à activer explicitement (`CMAKE_EXPERIMENTAL_SYCL`, `LANGUAGES SYCL`)"
  - "Le compilateur se choisit avec la variable d'environnement `SYCLCXX` : AdaptiveCpp, DPC++, Intel oneAPI ou Clang"
  - Les `FILE_SET` `SYCL` et `SYCL_HEADERS` propagent les besoins SYCL aux cibles qui en dépendent
  - "SYCL 2020 Rev. 13 : des en-têtes séparés (`sycl_khr_split_headers`) peuvent remplacer `<sycl/sycl.hpp>` pour compiler plus vite"
status: published
featured: false
edition: 2026-W47
---
Le groupe de travail SYCL de Khronos annonce que CMake reconnaît désormais SYCL comme un langage de projet à part entière. La prise en charge arrive avec CMake 4.5 : elle est expérimentale et doit être activée explicitement, en définissant la variable `CMAKE_EXPERIMENTAL_SYCL` avant l'appel à `project()`. On déclare ensuite le langage avec `project(MonProjet LANGUAGES CXX SYCL)`, ou avec `enable_language(SYCL)` dans un projet existant. Le développement a été financé par Intel et réalisé par Kitware. Le but est de réduire la logique de build sur mesure que chaque projet SYCL devait jusqu'ici maintenir.

CMake reconnaît automatiquement les fichiers `.sycl`, mais l'extension n'est pas obligatoire. Deux nouveaux types de `FILE_SET`, `SYCL` et `SYCL_HEADERS`, indiquent quelles sources et quels en-têtes doivent être compilés en SYCL. Si une cible utilise un ensemble `SYCL_HEADERS`, directement ou via une dépendance liée, CMake compile ses sources C++ en SYCL : une bibliothèque transmet ainsi ses besoins SYCL à ceux qui l'utilisent. Le `CMakeLists.txt` ne nomme aucune implémentation. Le compilateur se choisit lors de la première configuration avec la variable d'environnement `SYCLCXX`, et CMake le conserve ensuite pour ce répertoire de build. Les compilateurs reconnus sont `acpp` (AdaptiveCpp), `dpclang`/`dpclang++` (DPC++ open source), `icx`/`icpx` (oneAPI d'Intel) et `clang`/`clang++` avec `libsycl`. La prise en charge de SYCL dans Clang reste incomplète, mais elle suffit pour de petits exemples.

```bash
# Set variables to select the desired compilers.
$ export CXX=icpx
$ export SYCLCXX=icpx

# Invoke CMake to configure the build.
$ cmake -B build -S . -G Ninja

# Invoke CMake to perform the build.
$ cmake --build build --target all --verbose
```

L'article illustre le fonctionnement avec une addition de vecteurs dans un seul `main.cpp`, compilée sous Linux avec `icpx` et Ninja. Une nouvelle leçon de SYCLAcademy décrit le processus complet : installer une chaîne de compilation, configurer le projet, construire un exemple et expérimenter. Khronos attend des retours sur la détection des compilateurs, la compilation, l'édition de liens, les builds mixtes C++/SYCL et la combinaison de plusieurs projets.

Enfin, SYCL 2020 Revision 13 ajoute l'extension `sycl_khr_split_headers`. Elle fournit des en-têtes plus petits et ciblés, comme `index_space.hpp`, `queue.hpp` ou `usm.hpp`, à la place de `<sycl/sycl.hpp>`, qui reste disponible. Le gain sur les temps de compilation dépend de l'application et de l'implémentation.
