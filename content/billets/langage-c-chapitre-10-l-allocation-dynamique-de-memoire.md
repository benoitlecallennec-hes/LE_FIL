---
title: "Langage C, chapitre 10 : l'allocation dynamique de mémoire"
date: 2026-11-24T16:25:09.843Z
source: Cours Langage C
link: https://he-arc.github.io/1242.1-Langage_C/docs/cours/chapitre10cours/
tags:
  - c
lang: fr
level: beginner
format: article
reading_time: 9
summary: Le chapitre 10 du cours de langage C de la HE-Arc présente `malloc` et `free`. Il explique aussi comment un tableau 2D est rangé en mémoire et comment allouer puis libérer une matrice dynamique de type `int **`.
takeaways:
  - Allouer chaque chaîne à sa taille exacte avec `malloc` évite de réserver la taille maximale pour chacune.
  - Chaque `malloc` se teste contre `NULL` et se termine par un `free`, sinon le programme fuit de la mémoire.
  - Un tableau `int [2][3]` est contigu en mémoire et devient un `int (*)[3]` dans une expression, pas un `int **`.
  - "Une matrice dynamique `int **` s'alloue ligne par ligne et se libère dans l'ordre inverse : les lignes d'abord, puis le tableau de pointeurs."
status: rejected
draft: true
featured: false
---
Le chapitre comprend des slides, un squelette `chap10.c` à compléter pendant le cours, huit exemples commentés (10.01 à 10.08) et une série d'exercices. Le squelette fait afficher des tailles avec `sizeof` et lire dix phrases avec `fgets`, d'abord dans un tableau statique puis dans un tableau alloué dynamiquement. Il demande aussi d'allouer 4000 `char` avec `malloc` et d'observer ce qui se passe si l'on oublie d'inclure `<stdlib.h>`. Chaque exemple est compilé avec `gcc -Wall -Wextra -Wpedantic -Werror -std=c23`.

L'exemple 10.01 se demande comment mémoriser des phrases sans réserver la taille maximale pour chacune. Chaque phrase est lue dans un `buffer`, puis copiée avec `strcpy` dans un bloc alloué à sa taille exacte, qui compte le `'\n'` gardé par `fgets` et le `'\0'` final. L'exemple 10.02 montre une fuite mémoire : sans `free`, le programme consomme toute la mémoire avant que `malloc` ne renvoie `NULL`, ce qui peut fortement ralentir la machine.

Les exemples 10.03 à 10.05 portent sur les tableaux 2D statiques. Leurs éléments sont contigus et rangés ligne après ligne. Un tableau `m` de type `int [2][3]` devient un `int (*)[3]` dans une expression, et non un `int **`. On peut donc le parcourir à plat avec `int *ptr = (int *)m;` et `*(ptr + i)`. Les écritures `m[1][2]`, `(*(m + 1))[2]`, `*(m[1] + 2)` et `*(*(m + 1) + 2)` désignent le même élément.

L'exemple 10.06 alloue une matrice dynamique : d'abord un tableau de pointeurs, un par ligne, puis un bloc d'`int` pour chaque ligne. Chaque appel à `malloc` est suivi d'un test contre `NULL`. Les lignes allouées séparément ne sont pas forcément contiguës en mémoire. La matrice est libérée ligne par ligne avant le tableau de pointeurs.

```c
// Reserve an array with 3 pointers (one for each row)
matrix = malloc(sizeof(int *) * ROWS);
if (matrix == NULL) exit(EXIT_FAILURE);

// Reserve 5 columns per row
for (i = 0; i < ROWS; i++)
{
	matrix[i] = malloc(sizeof(int) * COLUMNS);
	if (matrix[i] == NULL) exit(EXIT_FAILURE);
}
```

Les deux derniers exemples dessinent une parabole dans un tableau `char space[80]` (10.07) et expliquent pourquoi lire une chaîne demande de la place réservée (10.08). Ils comparent un `char *` qui pointe sur un littéral à un tableau `char[100]` rempli avec `scanf("%99s")`.
