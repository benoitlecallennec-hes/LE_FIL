---
title: "Python free-threaded et fastsimdjson : lire du JSON à plus de 100 Go/s"
date: 2026-11-14T14:53:50.000Z
source: lemire.me
link: https://lemire.me/blog/2026/11/14/reading-json-at-277-gb-s-in-python/
tags:
  - autres
lang: en
level: intermediate
format: article
reading_time: 4
image: https://lemire.me/blog/wp-content/uploads/2026/11/superfastjson-feature-150x150.jpg
summary: Daniel Lemire montre qu'avec le build sans GIL de Python 3.14 et la bibliothèque multithreadée fastsimdjson, on peut analyser des fichiers NDJSON de plusieurs gigaoctets en quelques dizaines de millisecondes, jusqu'à 277 Go/s sur une machine de 192 cœurs.
takeaways:
  - Le build free-threaded de Python 3.14 (sans GIL) permet de paralléliser l'analyse JSON avec des threads plutôt qu'avec des processus.
  - fastsimdjson ne lit que les champs demandés par chemin JSON et appelle la fonction de l'utilisateur sur le thread qui a analysé l'enregistrement.
  - "Sur 192 cœurs : 138 Go/s pour compter les étoiles contre 116 Go/s pour le meilleur pool msgspec, et jusqu'à 277 Go/s pour extraire des valeurs."
  - L'avance sur msgspec passe de 3,5× sur un cœur à environ 20 % à pleine échelle, et elle varie selon les fichiers.
status: published
featured: false
edition: 2026-W47
---
Beaucoup de systèmes journalisent leurs données en NDJSON, un enregistrement JSON par ligne. GH Archive publie ainsi tous les événements publics de GitHub, soit environ un gigaoctet par heure. Daniel Lemire prend un exemple : compter les dépôts qui ont reçu le plus d'étoiles en huit heures, soit 7,6 Go et 2,2 millions d'événements. Le code Python évident, qui appelle `json.loads` sur chaque ligne, tourne à 0,28 Go/s et met 27 secondes. orjson va deux fois plus vite. msgspec, qui ne décode que les champs déclarés, va huit fois plus vite.

À cause du GIL, un processus Python n'exécute du code Python que sur un cœur à la fois. La solution habituelle est donc un pool de processus : on découpe le fichier en morceaux et chaque processus renvoie un petit tableau de comptes. Python 3.14 propose un build free-threaded (`python3.14t`) sans GIL, où les threads exécutent du code Python en parallèle. La bibliothèque fastsimdjson en profite. Sa fonction `map_many` reçoit les données, mappées en mémoire, une fonction Python et une liste de chemins JSON (par exemple `/type` et `/repo/name`). Elle analyse les enregistrements sur de nombreux threads, ne lit que ce qui mène aux champs demandés et appelle la fonction sur le thread qui a analysé l'enregistrement, avec ces seules valeurs. Le paramètre `drop_none` écarte les résultats vides.

```python
import collections
import fastsimdjson

def starred(event):
    kind, repo = event
    if kind == "WatchEvent":
        return repo

names = fastsimdjson.map_many(
    starred, data, threads=192,
    fields=["/type", "/repo/name"], drop_none=True)
stars = collections.Counter(names)
```

Les mesures ont été faites sur une instance AWS c8a.metal-48xl (deux AMD EPYC 9R45, 192 cœurs), avec Python 3.14.10, orjson 3.13 et msgspec 0.22. Sur un cœur, fastsimdjson lit 8 Go/s, soit 3,5 fois plus vite que msgspec. Avec 192 threads, il compte les étoiles des 7,6 Go en 55 ms, soit 138 Go/s. Le meilleur pool de processus msgspec plafonne à 116 Go/s avec 96 processus, et ralentit si on en ajoute. fastsimdjson reste devant quel que soit le nombre de workers, mais son avance passe de 3,5× à environ 20 %. Le comptage est d'ailleurs le cas le plus favorable aux processus, car peu de données remontent.

Pour un cas où l'on veut récupérer les valeurs elles-mêmes, l'auteur a extrait un ou deux champs de chaque enregistrement dans six fichiers d'environ 1 Go tirés du benchmark Pison. Sur le fichier twitter, fastsimdjson atteint 277 Go/s, et quatre fichiers sur six dépassent 100 Go/s. Sur les deux autres, l'un reste vers 20 Go/s (encore environ quatre fois msgspec) et l'autre est à égalité. Le code source est publié.
