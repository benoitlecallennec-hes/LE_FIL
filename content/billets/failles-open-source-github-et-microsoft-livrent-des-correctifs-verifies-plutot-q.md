---
title: "Failles open source : GitHub et Microsoft livrent des correctifs vérifiés plutôt que des rapports d'IA bruts"
date: 2026-11-17T18:29:29.000Z
source: The GitHub Blog
link: https://github.blog/open-source/maintainers/maintainers-dont-need-more-ai-bug-reports-they-need-fixes/
tags:
  - autres
feed_tag: Maintainers
lang: en
level: beginner
format: article
reading_time: 10
image: https://github.blog/wp-content/uploads/2026/11/01-participating-projects.png?resize=1024%2C576
summary: Avec Codename MDASH, GitHub et Microsoft ont réduit des milliers d'alertes produites par l'IA à 99 rapports privés vérifiés. Chacun était accompagné d'un correctif proposé et destiné à l'un de six projets open source. À la mi-novembre, 55 avaient déjà été corrigés.
takeaways:
  - "Des milliers d'alertes brutes de Codename MDASH (MAI Cyber) sont devenues 99 rapports privés vérifiés : 55 corrigés au 15 novembre, dont 14 déjà publiés dans une version."
  - Chaque rapport passe par un tri par IA (agent Copilot du GitHub Security Lab), puis par une vérification humaine, et arrive avec une preuve de concept et un correctif suggéré.
  - "La règle proposée : vérifier chaque constat, le signaler en privé, joindre un correctif et laisser les mainteneurs fixer le rythme."
  - La relecture humaine (sept chercheurs, environ deux jours par projet) ne passe pas encore à l'échelle.
missing:
  - balise og:image non vérifiée (image du corps de l'article utilisée)
status: published
featured: false
edition: 2026-W48
---
Grâce à l'IA, trouver des vulnérabilités potentielles est devenu rapide et bon marché. Les corriger demande toujours un jugement humain, et dans l'open source ce travail retombe sur les mainteneurs. Beaucoup reçoivent déjà une avalanche de signalements générés par IA, avec des doublons et des rapports qui ne décrivent aucune vraie faille. Ashley Wolf et Gregg Cochran défendent un principe : avant de lancer un modèle sur un projet, il faut demander l'accord de ses mainteneurs et venir avec des correctifs, pas seulement des constats. Le pilote mené par GitHub et Microsoft cherchait à savoir si l'IA pouvait aider à livrer des correctifs plutôt que d'ajouter des alertes.

Le processus compte cinq étapes. Codename MDASH, le harnais d'analyse agentique multi-modèles de Microsoft Security, analyse chaque projet avec le modèle MAI Cyber et propose des correctifs. Un agent de tri du GitHub Security Lab, construit sur des compétences Copilot adaptées, supprime les doublons, vérifie la validité et ne garde que les bugs à fort impact. Des chercheurs du Microsoft FORGE Lab vérifient ensuite chaque constat à la main. Les rapports arrivent enfin par le signalement privé de vulnérabilités de GitHub, avec une preuve de concept et un correctif suggéré. Les mainteneurs décident seuls de ce qu'ils corrigent, de la manière et du moment.

Six anciens projets du GitHub Secure Open Source Fund ont participé : AutoGPT, Caddy, Express, FastMCP, ImageMagick et Netty. Sur 9 791 problèmes possibles signalés, sept chercheurs ont retenu 99 rapports, soit environ un pour cent alertes brutes. Au 15 novembre, 55 étaient corrigés, dont 14 déjà publiés dans une version. Caddy a réglé ses 22 rapports en une semaine au plus. ImageMagick, une équipe de deux personnes, a poussé un correctif en 15 minutes. AutoGPT a reçu un rapport trois jours après avoir livré une vérification défectueuse : ses mainteneurs ont écrit en un jour un correctif plus poussé que celui proposé. Un mainteneur de Netty résume : trouver les vulnérabilités n'est plus le goulot d'étranglement, les corriger l'est.

Cette relecture manuelle, environ deux jours par projet, ne passe pas encore à l'échelle. Les auteurs demandent trois choses : des correctifs livrés via le signalement privé, des tests CI/CD sur des forks privés, et un accès aux modèles spécialisés en cybersécurité pour vérifier une preuve de concept avant et après le correctif. Ils conseillent aux mainteneurs d'activer le signalement privé, par exemple avec `gh-secure`. GitHub veut intégrer ces leçons au signalement privé et aux avis de sécurité, et proposer des analyses vérifiées à tous les anciens projets du Fund.
