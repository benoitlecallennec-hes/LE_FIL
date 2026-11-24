---
title: "datasette-agent 0.7a1 : un panneau latéral qui suit mieux la navigation par fragments d'URL"
date: 2026-11-24T03:45:57.000Z
source: Simon Willison
link: https://simonwillison.net/2026/Nov/24/datasette-agent/
tags: []
feed_tag: datasette-agent
lang: en
level: intermediate
format: article
reading_time: 1
summary: La version alpha 0.7a1 de Datasette Agent, l'agent de Simon Willison qui s'appuie sur un LLM, améliore la navigation de son panneau latéral `iframe` par fragments d'URL (`#…`). Cette navigation s'intègre ainsi mieux aux autres plugins Datasette.
takeaways:
  - Datasette Agent peut ouvrir un panneau latéral `iframe` pendant une session de discussion.
  - La version 0.7a1 améliore la navigation de ce panneau par fragments d'URL (`#…`).
  - Les plugins qui enregistrent leur état dans le fragment d'URL s'intègrent mieux à l'agent.
  - Ce changement a servi à corriger des bugs de datasette-charts 0.2a0.
missing:
  - image principale de la page (og:image non lue)
status: rejected
draft: true
featured: false
---
Datasette Agent est un agent conversationnel qui s'appuie sur un LLM et fonctionne avec Datasette. Pendant une session de discussion, il peut ouvrir un panneau latéral sous forme d'`iframe`, et ce panneau peut ensuite afficher d'autres pages.

La version 0.7a1 apporte une petite amélioration : le panneau suit mieux la navigation par fragments d'URL, c'est-à-dire la partie de l'adresse qui suit le `#`. Certains plugins Datasette modifient ce fragment quand l'utilisateur change leur état en interagissant avec eux. Ces plugins s'intègrent donc plus étroitement à l'agent.

Simon Willison explique avoir fait cette modification pour corriger des bugs de datasette-charts 0.2a0, une version qu'il juge bien plus intéressante et qui est sortie le même jour.
