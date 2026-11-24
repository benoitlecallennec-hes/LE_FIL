---
title: Votre modèle d'IA vous ment-il ? Le problème de la reproductibilité
date: 2026-11-21T15:00:32.000Z
source: CppCon
link: https://www.youtube.com/shorts/LxH_NtoewN0
tags:
  - ia
lang: en
level: beginner
format: short
image: https://i1.ytimg.com/vi/LxH_NtoewN0/hqdefault.jpg
summary: Un court extrait de CppCon explique pourquoi un modèle d'IA ne donne pas toujours le même résultat d'une exécution à l'autre, et pourquoi la reproductibilité reste un obstacle majeur en apprentissage automatique.
takeaways:
  - L'aléatoire et les processus stochastiques rendent les LLM non déterministes.
  - Une mise à jour de bibliothèque ou un changement d'environnement peut modifier les résultats.
  - Gérer les versions et l'environnement est indispensable pour reproduire un résultat.
  - Un modèle « boîte noire » est plus difficile à vérifier, donc à croire.
missing:
  - contenu de la vidéo (seule la description était disponible)
  - nom de l'intervenant
  - date de publication
status: selected
draft: true
featured: false
edition: 2026-W48
---
La vidéo part d'une question simple : obtient-on le même résultat chaque fois qu'on exécute un modèle d'IA ? Elle explique pourquoi les modèles « mentent », c'est-à-dire donnent des réponses incohérentes d'une exécution à l'autre, et quelles raisons techniques rendent leur comportement non déterministe.

Plusieurs causes sont abordées : la part d'aléatoire et de processus stochastiques dans les grands modèles de langage (LLM), puis l'environnement d'exécution et les mises à jour de bibliothèques, qui suffisent à changer les résultats.

La vidéo propose ensuite de bonnes pratiques de gestion de versions et de maîtrise de l'environnement pour l'IA. Elle explique aussi pourquoi les modèles « boîte noire » méritent moins de confiance qu'on ne le croit.
