---
title: Voir le code machine de son programme Java avec la bibliothèque instructive
date: 2026-11-23T13:00:38.000Z
source: lemire.me
link: https://lemire.me/blog/2026/11/23/seeing-the-machine-code-of-your-java-program/
tags:
  - autres
lang: en
level: advanced
format: article
reading_time: 9
image: https://lemire.me/blog/wp-content/uploads/2026/11/instructive-cover-150x150.jpg
summary: Daniel Lemire utilise `instructive`, la nouvelle bibliothèque de Richard Startin, pour voir instruction par instruction où le processeur passe son temps dans `RoaringBitmap.andCardinality`. Sur Apple M4, le même code est sept fois plus rapide avec le JDK 27 qu'avec le JDK 25.
takeaways:
  - "`instructive` échantillonne l'instruction machine en cours toutes les 10 ms, sans droits root ni outil externe."
  - Sur ARM avec le JDK 25, `Long.bitCount` passe par des copies entre registres scalaires et vectoriels qui rendent chaque mot dépendant du précédent.
  - Le JDK 27 vectorise la boucle d'`andCardinality` et la rend sept fois plus rapide sur Apple M4.
  - Sur Xeon avec AVX-512, Java utilise `vpopcntq` sur huit mots à la fois, soit 0,15 ns par mot.
status: rejected
draft: true
featured: false
---
JMH dit combien de temps prend une fonction, mais pas pourquoi elle est lente. Pour le savoir, il faut voir le code machine produit par le compilateur JIT et l'endroit où le processeur passe son temps. La bibliothèque `instructive` de Richard Startin note toutes les 10 ms de temps CPU quelle instruction machine s'exécute. Elle donne ensuite les méthodes compilées les plus chaudes, avec le nombre d'échantillons par instruction. Il suffit d'entourer le travail mesuré par `Sampler.start()` et `Sampler.stop()` et d'ajouter le jar avec `-javaagent`. Lemire l'applique à `andCardinality`, la fonction de RoaringBitmap qui compte les éléments communs à deux ensembles sans construire leur intersection. Quand les deux morceaux sont des conteneurs bitmap, tout se joue dans cette boucle :

```java
public int andCardinality(final BitmapContainer value2) {
  int newCardinality = 0;
  for (int k = 0; k < this.bitmap.length; ++k) {
    newCardinality += Long.bitCount(this.bitmap[k] & value2.bitmap[k]);
  }
  return newCardinality;
}
```

Le test porte sur deux ensembles denses de 64 morceaux de 65 536 entiers, soit 65 536 mots par appel. La méthode est d'abord appelée 10 000 fois pour que C2 la compile, puis 100 000 fois sous échantillonnage. Sur Apple M4 avec le JDK 25, un appel prend 115 µs, soit environ 8 cycles par mot. Avant l'extension CSSC, ARM n'a pas d'instruction de comptage de bits pour les registres 64 bits. Java copie donc chaque mot dans un registre vectoriel, applique `cnt` puis `addv`, et recopie le résultat. Environ 80 % des échantillons tombent sur ces deux copies. La première n'écrit que la moitié basse de `v16` et dépend donc du comptage précédent : chaque mot attend le précédent. En C avec de l'assembleur en ligne, une instruction qui écrit tout le registre (`fmov`) rend la même séquence six fois plus rapide.

Avec le JDK 27, l'appel tombe à 15,6 µs, sept fois plus vite. La boucle est vectorisée : elle charge deux mots à la fois dans des registres de 128 bits (`ldr q`) et ne fait plus de copies entre registres scalaires et vectoriels. Le code produit pour `Long.bitCount` n'a pas changé. Quand la boucle n'est pas vectorisée, elle reste aussi lente qu'avec le JDK 25. Sur un Xeon Gold 6548N (Emerald Rapids), le JDK 25 comme le JDK 27 utilisent AVX-512. Chaque itération charge huit mots, applique `vpandd`, compte leurs bits d'un seul `vpopcntq`, puis cumule avec `vpmovqd` et `vpaddd`. Un appel prend 10 µs, soit 0,15 ns par mot : plus rapide que l'Apple M4 quelle que soit la version de Java.

Le `perfasm` de JMH donne un résultat comparable. Il demande toutefois un projet JMH, le désassembleur `hsdis` et un profileur système (`perf` sous Linux, DTrace avec les droits root sous macOS). `instructive` tient en trois lignes dans un programme ordinaire et s'utilise sans droits root, dans une vraie application sur des données réelles. Le profil est un objet Java qu'on peut trier et filtrer. Des règles signalent aussi les longues chaînes de dépendances et les boucles non déroulées.
