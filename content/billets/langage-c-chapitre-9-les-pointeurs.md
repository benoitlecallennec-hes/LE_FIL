---
title: "Langage C, chapitre 9 : les pointeurs"
date: 2026-11-24T16:25:09.843Z
source: Cours Langage C
link: https://he-arc.github.io/1242.1-Langage_C/docs/cours/chapitre09cours/
tags:
  - c
lang: fr
level: beginner
format: article
reading_time: 10
summary: Le chapitre 9 du cours de langage C de la HE-Arc porte sur les pointeurs. On y voit l'adresse d'une variable, le déréférencement, la fonction `swap`, les différences entre tableaux et pointeurs, plusieurs versions de `strcpy()` et l'arithmétique des pointeurs.
takeaways:
  - Pour qu'une fonction comme `swap` modifie les variables de l'appelant, il faut lui passer leurs adresses (`int *a`, `int *b`).
  - "`t + SIZE` pointe juste après le dernier élément : on peut le comparer, pas le déréférencer."
  - La différence entre deux pointeurs (`&t[3] - p`, affichée avec `%td`) compte des éléments, pas des octets.
  - "Avec `-Wall`, `gcc` refuse la forme courte `while (*p++ = *q++);` (`-Wparentheses`) : la comparaison à `'\\0'` doit être écrite."
missing:
  - image principale de la page
status: selected
draft: true
featured: false
edition: 2026-W48
notions:
  - Pointeurs
---
Le chapitre commence par des slides imprimables et un squelette `chap9.c` à compléter pendant le cours. On y affiche les adresses des variables, on déclare plusieurs pointeurs sur une même ligne pour voir quel type reçoit chaque variable, on modifie des valeurs uniquement par des pointeurs et on teste un pointeur initialisé à `NULL` (en C23, `nullptr`) avant de s'en servir.

Six exemples commentés suivent, chacun avec son fichier source et sa sortie : obtenir l'adresse d'une variable et la modifier avec un pointeur, échanger deux valeurs avec une fonction `swap(int *a, int *b)`, comparer tableaux et pointeurs, voir si l'on peut affecter une nouvelle chaîne à un tableau passé en paramètre, écrire sa propre version de `strcpy()`, et parcourir un tableau avec un pointeur. Pour ce dernier exemple, le cours précise que `t + SIZE` désigne la case juste après le dernier élément : on peut le comparer, jamais le déréférencer. Il précise aussi que la différence de deux pointeurs, affichée avec `%td`, compte des éléments, pas des octets.

Les versions de `mystrcpy` remplacent peu à peu l'indice par des pointeurs qui avancent. La dernière fait la copie et l'incrémentation dans la condition de la boucle :

```c
// 4) COPY AND INCREMENT IN THE CONDITION
// Short form: while (*ptrCh1++ = *ptrCh2++);
// gcc -Wall rejects it (-Wparentheses): the comparison has to be explicit.
void mystrcpy_v4(char *ptrCh1, const char *ptrCh2)
{
	while ((*ptrCh1++ = *ptrCh2++) != '\0')
		;
}
```

Le chapitre se termine par douze exercices, dont `ch09_ex06_CopyArrays()` et `ch09_ex10_Swap()`, puis par dix-sept exercices d'autoévaluation ARGOS : six faciles et onze de difficulté moyenne.
