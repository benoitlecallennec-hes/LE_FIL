---
title: "datasette-charts 0.1a0 : des graphiques Datasette calculés sur toutes les lignes, pas seulement la page affichée"
date: 2026-11-23T00:37:20.000Z
source: Simon Willison
link: https://simonwillison.net/2026/Nov/23/datasette-charts/
tags:
  - ia
feed_tag: generative-ai
lang: en
level: intermediate
format: article
reading_time: 2
image: https://static.simonwillison.net/static/2026/datasette-charts-0.1a0.webp
summary: Simon Willison publie la première alpha de `datasette-charts`, un plugin de graphiques pour Datasette dont il a construit le prototype avec Claude Fable 5.2. Le plugin propose des graphiques adaptés aux données et compile chacun d'eux en une requête SQL d'agrégation qui porte sur l'ensemble des résultats.
takeaways:
  - "`datasette-charts` suggère des graphiques à partir d'heuristiques propres à chaque type de graphique (D3, Observable Plot, `topojson-client`)."
  - Chaque graphique devient une requête SQL d'agrégation (`GROUP BY`, tranches de temps, fonctions de fenêtrage) calculée sur tous les résultats, plafonnée à 5 000 lignes renvoyées.
  - Les suggestions s'appuient sur un échantillon de lignes, mais les graphiques eux-mêmes utilisent toutes les lignes.
  - L'état du graphique est stocké dans le `#fragment` de l'URL, ce qui permet de le mettre en favori ou de le partager.
status: rejected
draft: true
featured: false
---
Simon Willison réfléchissait à ce problème depuis plus d'un an. Un dimanche, il a demandé à Claude Fable 5.2 de construire un premier prototype, qui a si bien fonctionné qu'il est devenu un plugin complet, `datasette-charts` 0.1a0. On peut l'essayer dans Datasette Lite. Il fonctionne sur n'importe quelle table et sur n'importe quelle requête SQL de Datasette. Il définit plusieurs types de graphiques, qui s'appuient sur des copies intégrées de D3, d'Observable Plot et de `topojson-client`. Chaque type est associé à des heuristiques qui jugent s'il convient à un ensemble de lignes donné.

En haut du panneau, des vignettes de suggestion montrent de vraies visualisations tirées des données, par exemple le total des téléchargements par semaine, ventilé par paquet. Cliquer sur l'une d'elles ouvre le graphique en grand avec des outils pour le modifier : type de graphique, axe du temps, valeur agrégée, découpage en séries avec regroupement des autres valeurs dans « Other ». On peut aussi construire un graphique de zéro. Comme dans les autres plugins de visualisation de Datasette, l'état du graphique est enregistré dans un `#fragment` de l'URL, ce qui permet de le mettre en favori ou de le partager.

Le point essentiel est que les graphiques tiennent compte de toutes les lignes. Les plugins précédents, comme `datasette-vega`, ne traçaient que la page de résultats affichée, en général 100 lignes au plus. Le nouveau plugin compile chaque graphique en une requête SQL qui agrège l'ensemble des résultats, parfois des millions de lignes, à l'aide de `GROUP BY`, d'un regroupement par tranches de temps et de fonctions de fenêtrage. Chaque requête renvoie au plus 5 000 lignes, une quantité adaptée à l'affichage. Pour proposer des graphiques sur les grandes tables sans évaluer tous les résultats, le plugin travaille sur un échantillon.

L'auteur précise que le plugin est encore une alpha. Selon lui, c'est ce qui se rapproche le plus de son idéal : que Datasette aide à visualiser les données en demandant le moins d'indications possible à l'utilisateur.
