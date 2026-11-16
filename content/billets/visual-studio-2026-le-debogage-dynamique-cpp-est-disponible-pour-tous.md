---
title: "Visual Studio 2026 : le débogage dynamique C++ est disponible pour tous"
date: 2026-11-12T16:49:13.000Z
source: Microsoft C++ Team Blog
link: https://devblogs.microsoft.com/cppblog/c-dynamic-debugging-is-now-generally-available/
tags:
  - cpp
feed_tag: C++
lang: en
level: intermediate
format: article
reading_time: 3
image: https://devblogs.microsoft.com/cppblog/wp-content/uploads/sites/9/2025/05/image-before.png
summary: Microsoft annonce la disponibilité générale de C++ Dynamic Debugging dans tous les canaux de Visual Studio 2026 et dans les MSVC Build Tools 14.52. L'outil permet de déboguer une build optimisée en désoptimisant uniquement les fonctions inspectées.
takeaways:
  - Disponible dans tous les canaux de Visual Studio 2026 et dans les MSVC Build Tools 14.52
  - Seules les fonctions où l'on pose un point d'arrêt ou dans lesquelles on entre sont désoptimisées, le reste du programme garde ses performances
  - S'active avec l'option de projet MSBuild ou le commutateur /dynamicdeopt (cl, lib, link) et désactive la Whole Program Optimization
  - Compatible avec Unreal Engine 5.6+, FastBuild et IncrediBuild
status: rejected
draft: true
featured: false
---
C++ Dynamic Debugging est désormais en disponibilité générale dans tous les canaux de Visual Studio 2026 et dans les Microsoft C++ (MSVC) Build Tools 14.52. Jusqu'ici, les développeurs de jeux ou de grosses applications devaient choisir. Une build de débogage donne accès aux symboles mais coûte en performances. Une build optimisée reste rapide, mais certaines variables y sont « optimisées » et le pas-à-pas devient peu fiable.

Avec Dynamic Debugging, on peut inspecter les variables locales, avancer ligne par ligne et entrer dans les fonctions inlinées, pendant que le reste de l'application reste optimisé. Une fois l'option activée et le projet recompilé, le débogueur désoptimise automatiquement une fonction quand on y place un point d'arrêt ou qu'on y entre en pas-à-pas. Plus besoin d'ajouter #pragma optimize, de recompiler puis de reproduire le problème. Des équipes Microsoft l'utilisent déjà, notamment chez Havok et dans la réalité virtuelle, et l'article cite deux ingénieurs principaux qui saluent la fin du compromis entre performances réalistes et confort de débogage.

Dans un projet MSBuild x64, il faut sélectionner la configuration Release puis régler Configuration Properties > Advanced > Use C++ Dynamic Debugging sur Yes, et recompiler. Ce réglage désactive la Whole Program Optimization. Pour CMake et les autres systèmes de build, on passe le nouveau commutateur /dynamicdeopt à cl.exe, lib.exe et link.exe. Unreal Engine 5.6 et suivants le prennent en charge via WindowsPlatform.bDynamicDebugging = true; dans le fichier .Target.cs. L'outil fonctionne aussi avec les builds distribuées FastBuild et IncrediBuild. Les prérequis et les limitations sont décrits dans la documentation.
