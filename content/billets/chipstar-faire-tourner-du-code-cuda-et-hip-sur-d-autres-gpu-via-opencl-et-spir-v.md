---
title: "chipStar : faire tourner du code CUDA et HIP sur d'autres GPU via OpenCL et SPIR-V"
date: 2026-11-10T14:13:00.000Z
source: khronos.org
link: https://www.khronos.org/blog/chipstar-brings-cuda-and-hip-applications-to-more-hardware-with-opencl-and-spir-v
tags:
  - 3d
lang: en
level: intermediate
format: article
reading_time: 5
image: https://www.khronos.org/images/jcogs_img/cache/crimson_chipstar_banner_with_tech_logos_-_28de80_-_ade52cc9e2371411e2f6a5e18fb8664a9767175d.png
summary: Le projet open source chipStar compile des kernels CUDA et HIP en SPIR-V puis les exécute via OpenCL. Des applications scientifiques peuvent ainsi changer de matériel sans être réécrites pour chaque plateforme.
takeaways:
  - chipStar compile des kernels CUDA et HIP en SPIR-V et les exécute via OpenCL ou Level Zero, sans réécriture par plateforme.
  - "Matériel ciblé : CPU et GPU AMD, Arm, Apple M4, Intel, NVIDIA et RISC-V, si le pilote OpenCL prend en charge SPIR-V."
  - "`hipBLAS`, `hipFFT` et `hipSOLVER` fonctionnent sur GPU Intel grâce à oneMKL ; OpenMM, libCEED et GAMESS-GPU-HF ont tourné sur Aurora."
  - "Points à améliorer dans les standards : synchronisation entre workgroups, command graphs, barrières avec sortie anticipée de threads."
status: rejected
draft: true
featured: false
---
Les applications scientifiques dépendent souvent d'API GPU propriétaires, si bien qu'un changement de matériel impose un changement de code. chipStar, un projet open source fondé sur une chaîne d'outils Clang/LLVM, compile les kernels CUDA et HIP pris en charge en SPIR-V. Il les exécute ensuite sur les périphériques compatibles via OpenCL, ou via Level Zero, le runtime bas niveau oneAPI d'Intel, proposé comme autre backend. Les développeurs conservent l'essentiel de leur code et concentrent le portage sur trois points : les fonctionnalités non prises en charge, les dépendances aux bibliothèques et l'optimisation des performances.

Les plateformes visées sont des CPU et des GPU d'AMD, Arm, Apple (M4), Intel et NVIDIA, ainsi que des processeurs RISC-V. Il faut toutefois que le pilote OpenCL offre la prise en charge de SPIR-V et les fonctionnalités nécessaires. Côté bibliothèques, chipStar porte des primitives parallèles, des algorithmes, de la génération de nombres aléatoires et des bibliothèques de matrices creuses. Sur GPU Intel, il intègre aussi `hipBLAS`, `hipFFT` et `hipSOLVER`, implémentées au-dessus d'Intel oneMKL.

Trois cas présentés à IWOCL 2026, tous exécutés sur le supercalculateur Aurora d'Argonne, illustrent la démarche. Pour OpenMM (dynamique moléculaire), le chemin HIP a tourné sur des GPU Intel Ponte Vecchio, au prix de modifications du système de build, du remplacement d'opérations propres au matériel et d'un réglage de la communication entre threads. Pour libCEED (opérateurs d'éléments finis), il a fallu adapter l'intégration au build et corriger une synchronisation dans un kernel ; les temps mesurés avec les backends OpenCL et Level Zero étaient très proches dans la configuration testée. Pour GAMESS-GPU-HF (chimie quantique, Hartree–Fock), la charge HIP a été activée sur GPU Intel en raccordant les appels mathématiques à oneMKL.

Ce travail fait aussi remonter des besoins vers les standards : la synchronisation entre workgroups, les command graphs et le comportement des barrières quand certains threads se terminent tôt. Les groupes de travail Khronos étudient ces sujets.
