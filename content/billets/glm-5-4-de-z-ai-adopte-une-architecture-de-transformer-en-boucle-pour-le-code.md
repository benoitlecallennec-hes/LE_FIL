---
title: GLM-5.4 de Z.ai adopte une architecture de transformer en boucle pour le code
date: 2026-11-23T23:40:49.000Z
source: Simon Willison
link: https://simonwillison.net/2026/Nov/23/glm-54/
tags:
  - ia
feed_tag: ai
lang: en
level: intermediate
format: article
reading_time: 2
image: https://static.simonwillison.net/static/2026/glm-5.4-max.webp
summary: Le laboratoire chinois Z.ai sort GLM-5.4, son premier modèle à architecture de transformer en boucle. Simon Willison l'a essayé via l'API de l'offre Coding Plan, a écrit le plugin `llm-glm` et compare les pélicans de quatre générations de GLM.
takeaways:
  - "Un Loop Transformer fait passer plusieurs fois les états cachés dans un groupe de couches aux poids partagés : le réseau gagne en profondeur sans paramètres supplémentaires."
  - "Inkling 2 Mini, de Thinking Machines Lab, utilise aussi cette approche : il exécute trois fois les couches du milieu pour chaque token."
  - GLM-5.4 n'a pas de poids ouverts et n'est accessible que par l'abonnement Coding Plan, via l'API Responses compatible OpenAI à `https://api.z.ai/api/v1`.
  - Le plugin `llm-glm` permet d'utiliser GLM-5.4 depuis LLM en attendant que `llm-zai` le prenne en charge.
status: selected
draft: true
featured: false
edition: 2026-W48
---
GLM-5.4 est le premier modèle de Z.ai fondé sur un « Loop Transformer ». Z.ai le décrit ainsi : dans un Transformer classique, chaque couche s'exécute une seule fois par passe avant ; dans un Loop Transformer, un groupe de couches aux poids partagés s'exécute sur plusieurs itérations et affine les états cachés de la passe précédente. Le réseau gagne ainsi en profondeur sans que le nombre de paramètres augmente. Simon Willison note que c'est la deuxième fois en une semaine qu'il voit passer cette architecture. Le 20 novembre, Thinking Machines Lab présentait Inkling 2 Mini, qui exécute trois fois les couches du milieu du modèle pour chaque token. Selon ce laboratoire, le modèle raisonne davantage et trouve la bonne réponse avec moins de tokens, surtout sur des tâches comme le codage agentique, qui dépendent de la qualité du raisonnement.

Les poids de GLM-5.4 ne sont pas (encore ?) publiés. Le modèle n'est pas accessible par l'API ordinaire, et Simon Willison a dû souscrire l'abonnement Coding Plan de Z.ai pour l'essayer. Il a constaté que l'offre Coding Plan et l'API ordinaire partagent la même API Responses compatible OpenAI, à l'adresse `https://api.z.ai/api/v1`. La clé du Coding Plan fonctionne avec cette API et donne accès au nouveau modèle.

Il a écrit le plugin `llm-glm` pour son outil LLM, puis découvert que le plugin `llm-zai` de Tommy Hughes existait déjà. Il a quand même publié le sien, parce que `llm-zai` ne prend pas encore en charge GLM-5.4. Il compte le retirer dès que `llm-zai` aura rattrapé son retard.

Il a soumis à GLM-5.4 son test habituel, le dessin d'un pélican à vélo, avec les niveaux d'effort low, high et max, puis a fait de même avec GLM-5.3. Les deux versions donnent des résultats de qualité comparable. Pour lui, les pélicans de GLM-5.1 et de GLM-5.2 avaient un style propre, très différent de celui des autres modèles, alors que ceux de 5.3 et 5.4 ressemblent beaucoup plus à ce que produisent les modèles d'OpenAI ou d'Anthropic. Une grille compare les quatre générations, et le changement de style entre 5.2 et 5.3 y est net.
