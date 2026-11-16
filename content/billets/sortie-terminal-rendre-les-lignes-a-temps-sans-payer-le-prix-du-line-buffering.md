---
title: "Sortie terminal : rendre les lignes à temps sans payer le prix du line buffering"
date: 2026-11-05T00:25:25.000Z
source: nullprogram.com
link: https://nullprogram.com/blog/2026/11/05/
tags:
  - c
feed_tag: c
lang: en
level: intermediate
format: article
reading_time: 7
summary: Chris Wellons montre que le *line buffering* appliqué par défaut aux terminaux ralentit les programmes qui affichent beaucoup de lignes. Il propose de vider le tampon au bout d'un court délai plutôt qu'à chaque `'\n'`, à la manière de l'algorithme de Nagle.
takeaways:
  - Sur un terminal, le *line buffering* peut dominer le temps d'exécution d'un outil qui affiche beaucoup de lignes, comme `unzip` sans `-q`.
  - glibc fait un `write` par ligne, même au milieu d'une chaîne. CPython regroupe les lignes, mais peut écrire des lignes partielles.
  - Mieux vaut vider le tampon après un court délai d'inactivité, comme le fait l'algorithme de Nagle, plutôt qu'à chaque `'\n'`.
  - "La vérification de l'échéance (`deadline_flush()`) se fait entre deux unités de travail, sans thread : on garde l'affichage immédiat avec le débit d'un tampon complet."
status: published
featured: false
edition: 2026-W47
---
Quand la sortie va vers un terminal, la bibliothèque C passe en *line buffering* et vide son tampon à chaque ligne terminée. Vers un fichier ou un pipe, elle bufferise entièrement. Sur un programme non interactif, cette différence suffit à créer un goulot d'étranglement. L'auteur prend l'exemple d'Info-ZIP `unzip` : sur une archive contenant beaucoup de petits fichiers, l'affichage des noms prend l'essentiel du temps d'extraction, au point qu'il vaut mieux interrompre puis relancer avec `-q`. Son propre outil, tugz unzip, garde le même affichage sans en payer le coût.

Le comportement varie selon les implémentations. Un `strace` sur un programme qui fait `puts("a\nb")` puis `puts("c")` montre que glibc prend le *line buffering* au pied de la lettre : il fait un `write` par ligne, quitte à découper une même chaîne. CPython applique une autre règle : il vide le tampon s'il contient un saut de ligne après l'ajout. Il regroupe donc `"a\nb\n"` en un seul appel, mais peut émettre des lignes partielles. L'auteur reconstruit ensuite un tampon `Buf` en C (`buf`, `cap`, `len`, `fd`, `flags` avec `BUF_ERR` et `BUF_LINE`), avec une fonction `flush()` et un `append()` qui applique la stratégie « tester puis vider » grâce à `has_newline()`. Afficher une série de lignes coûte alors autant d'appels système qu'il y a de lignes, là où un seul aurait suffi.

Pour lui, le but n'est pas d'écrire à chaque saut de ligne, mais de ne jamais retenir indéfiniment des lignes destinées au terminal. Il reprend l'idée de l'algorithme de Nagle : vider le tampon après un court délai, sans s'occuper des sauts de ligne. Cela demande de modifier un peu l'interface, en ajoutant au tampon une échéance lue sur une horloge monotone :

```c
typedef struct {
    int64_t deadline;
    // ...
} Buf;

int64_t os_monotonic_clock();
```

Une fonction `deadline_flush()` vide le tampon quand l'échéance est dépassée. Le délai visé, de l'ordre de 100 ms, reste quasi imperceptible. Il n'y a pas de thread en arrière-plan : la vérification se fait entre deux petites unités de travail, par exemple entre deux blocs de décompression dans tugz. Les noms de tous les fichiers traités dans une même fenêtre partent en un seul `write`. L'utilisateur voit le même affichage, mais le programme va plus vite.
