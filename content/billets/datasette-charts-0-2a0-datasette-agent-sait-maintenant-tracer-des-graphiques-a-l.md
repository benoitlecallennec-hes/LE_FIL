---
title: "datasette-charts 0.2a0 : Datasette Agent sait maintenant tracer des graphiques à la demande"
date: 2026-11-24T04:24:29.000Z
source: Simon Willison
link: https://simonwillison.net/2026/Nov/24/datasette-charts/
tags: []
feed_tag: datasette-agent
lang: en
level: intermediate
format: article
reading_time: 1
summary: Au lendemain de sa présentation, le plugin de graphiques `datasette-charts` passe en version `0.2a0` et s'intègre à Datasette Agent. On demande un graphique dans le chat et l'agent le construit avec les mêmes outils de suggestion et de génération.
takeaways:
  - "`datasette-charts` `0.2a0` donne à Datasette Agent un outil pour tracer un graphique dans le chat et un autre pour suggérer des graphiques sur une table ou une requête."
  - Le graphique peut s'ouvrir dans un panneau du constructeur de graphiques, à côté de la page de la table.
  - L'agent voit les changements faits dans le panneau et peut décrire le graphique affiché.
  - Nouvelle unité de temps `decade`, et courbes continues même quand certaines périodes n'ont pas de données.
missing:
  - contenu de la vidéo de démonstration (seule la description de l'article a été lue)
status: rejected
draft: true
featured: false
---
Simon Willison avait présenté la veille `datasette-charts`, un nouveau plugin de graphiques pour Datasette. La version `0.2a0` l'intègre à Datasette Agent : on peut demander un graphique dans le chat, et l'agent le crée avec les mêmes outils de suggestion et de génération de graphiques que le plugin. D'après les notes de version, quand les deux plugins sont installés, l'agent reçoit deux outils : l'un affiche un graphique dans la conversation, avec un lien vers le constructeur de graphiques, l'autre suggère des graphiques pour une table ou une requête.

Une vidéo de démonstration a été tournée sur agent.datasette.io, où l'on peut essayer le plugin en se connectant avec GitHub. L'auteur y demande « Use #stats to show sqlite-utils downloads over time as a chart » ; taper `#stats` ouvre une recherche de table. Le graphique s'affiche dans le chat, et le lien « Open in the chart builder » l'ouvre dans un panneau, à côté de la page de la table.

Depuis la page de la table, l'auteur remplace ensuite le graphique par celui des téléchargements de `datasette`. Quand il demande à l'agent ce que montre le graphique, celui-ci remarque que le contenu du panneau a changé et décrit le nouveau graphique. Un export Markdown de la conversation accompagne la vidéo.

La version apporte aussi une nouvelle unité de temps, `decade`, proposée pour les colonnes qui couvrent plusieurs siècles, quand un regroupement par année donnerait trop de valeurs. Enfin, les courbes dont certaines périodes n'ont pas de données, par exemple une ligne par décennie regroupée par année, sont maintenant tracées d'un trait continu plutôt qu'en points isolés.
