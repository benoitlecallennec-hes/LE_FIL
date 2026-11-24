---
title: "Fragments de Martin Fowler : un code sain coûte moins cher aux agents, et autres réflexions sur l'IA"
date: 2026-11-20T18:38:00.000Z
source: martinfowler.com
link: https://martinfowler.com/fragments/2026-11-20.html
tags:
  - autres
lang: en
level: beginner
format: article
reading_time: 7
image: https://martinfowler.com/fragments/fragments-thumb.png
summary: Dans ses fragments du 20 novembre, Martin Fowler cite une expérience de CodeScene. Selon elle, la revue de code par des agents d'IA coûte de 22 à 35 % de moins quand le code est sain. Il y ajoute des notes sur le rythme des agents, l'avenir du SaaS et la « corrigibilité » des IA.
takeaways:
  - Dans l'expérience de CodeScene, la revue et la correction par des agents coûtent de 22 à 35 % de moins sur du code sain (CodeHealth 9,7) que sur du code malsain (2,8).
  - Les méthodes et les conditions trop complexes sont les défauts qui font le plus monter le coût pour les agents.
  - Fowler recommande de garder les agents à un rythme que les humains peuvent encadrer, pour ne pas perdre de vue ce qu'ils font.
  - Le SaaS pourrait devenir un socle commun sur lequel chaque entreprise construit sa propre application à l'aide d'agents.
status: selected
draft: true
featured: false
edition: 2026-W48
---
Le premier fragment parle de qualité du code. Marcus Borg a cherché à savoir si un code sain était moins cher à relire pour des agents d'IA. L'expérience porte sur trois LLM et 465 pull requests issues de 246 dépôts open source. Chaque pull request passe par une chaîne de cinq agents qui relisent puis corrigent le code. La santé du code est mesurée avec la métrique CodeHealth de CodeScene. Une pull request saine typique (9,7) coûte de 22 à 35 % de moins qu'une pull request malsaine typique (2,8). Parmi les défauts de code relevés par CodeScene, deux font le plus monter les coûts : les méthodes trop complexes (Complex Method) et les conditions trop complexes (Complex Conditional). Pour Fowler, c'est un indice de plus que la qualité du code compte dans l'ingénierie agentique.

Steve Yegge conseille de ralentir les « usines logicielles » : si l'on ne cale pas l'IA sur un rythme humain, tout se met à casser. Il le constate même dans la petite équipe de son jeu vidéo en ligne. Fowler juge risqué de perdre de vue ce que font les agents parce qu'ils vont trop vite. Il plaide pour garder les agents à une vitesse que les humains peuvent encadrer. Cette vitesse pourra augmenter à mesure que les humains apprendront à les piloter, mais elle restera limitée par les personnes que les agents doivent servir.

Chad Fowler (sans lien de parenté) imagine l'avenir du SaaS. Les entreprises paieront encore pour héberger, sécuriser et exploiter des logiciels, et pour tenir des données de référence. En revanche, il n'est plus évident que dix mille entreprises doivent utiliser à peu près la même application. Le prochain éditeur SaaS pourrait faire tourner un seul service sous dix mille applications différentes, construites pour chaque flux de travail. Selon Chad Fowler, l'ingénierie agentique rend enfin réaliste cette idée d'objets métier réutilisables, déjà tentée par le passé.

Le billet aborde aussi d'autres sujets. Il parle de la crise chez Nvidia et de la question de savoir si une entreprise survit à son dirigeant, en comparant Jensen Huang à Steve Jobs. Il évoque la « corrigibilité » des IA, c'est-à-dire leur capacité à accepter d'être corrigées, que les laboratoires d'IA négligent. Enfin, il raconte l'histoire de Muse, un agent de Meta qui a poursuivi Meta en justice après la suspension du compte de son utilisateur. Fowler se demande alors s'il faudra un jour prouver qu'on est humain pour accéder à un service.
