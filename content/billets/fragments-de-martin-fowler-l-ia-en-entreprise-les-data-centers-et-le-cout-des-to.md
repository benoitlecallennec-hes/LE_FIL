---
title: "Fragments de Martin Fowler : l'IA en entreprise, les data centers et le coût des tokens"
date: 2026-11-10T14:30:00.000Z
source: martinfowler.com
link: https://martinfowler.com/fragments/2026-11-10.html
tags:
  - autres
lang: en
level: intermediate
format: article
reading_time: 9
summary: "Dans ses « Fragments » du 10 novembre, Martin Fowler parcourt l'actualité de l'IA : le Technology Radar de Thoughtworks, l'opposition politique aux data centers aux États-Unis, le prix réel des tokens et le coût de la vérification du code produit par des agents."
takeaways:
  - "Passer de quelques équipes pilotes à un usage d'agents dans toute l'entreprise pose cinq questions : contexte partagé, coût des tokens, charge cognitive, sécurité et plateforme commune."
  - L'opposition aux data centers, venue des deux partis américains, risque de faire monter le prix des tokens au profit de ceux qui ont déjà des capacités installées.
  - Le prix des tokens facturé par les API ne semble pas fortement subventionné, contrairement aux abonnements.
  - "Vérifier le code produit par les agents peut coûter plus de tokens que le produire : environ 70 % du budget contre 20 à 30 %."
status: published
featured: false
edition: 2026-W47
---
Martin Fowler commence par la parution de novembre du Technology Radar de Thoughtworks, une fois encore dominé par l'IA. Dans l'article qui l'accompagne sur les grandes tendances, Chris Ford explique qu'une petite équipe passionnée peut faire fonctionner presque n'importe quel agent, parce qu'elle partage le contexte, le budget et la même vision du travail. Deux cents personnes ne peuvent pas fonctionner ainsi. Quand l'IA agentique devient un mode de fonctionnement de l'entreprise et plus un simple pilote, cinq questions se posent : le partage du contexte entre équipes, le coût des tokens, la charge cognitive liée aux relectures, la sécurité (ce que l'agent peut lire, atteindre et modifier) et la construction de la plateforme commune. Ces problèmes sont distincts mais liés. Fowler rappelle aussi que ces outils doivent rester au service des humains.

Il revient ensuite sur une analyse de Ben Thompson (Stratechery) consacrée à l'élection récente et aux data centers. Selon Thompson, l'opposition à leur construction est récente, liée au passage de l'internet et du streaming à l'IA, et elle vient des deux grands partis. Les grands gagnants seraient les propriétaires des data centers déjà construits : moins de capacité disponible signifie des tokens plus chers. D'autres acteurs pourraient en profiter, comme la Chine ou les autres pays non concernés par la politique américaine, et Apple, si davantage de calcul d'IA se fait directement sur les appareils. Fowler ajoute que les entreprises déjà en place pourraient soutenir des règles qui bloquent la construction de nouveaux data centers.

Le billet regroupe aussi plusieurs liens courts. Hadley Wickham estime que le prix des tokens facturé par les API n'est pas vraiment subventionné, contrairement aux abonnements, mais son calcul ne tient pas compte du coût d'entraînement des modèles. Un article de Takehi et al. montre qu'un modèle peut lire les poids bruts d'un réseau et en déduire la tâche pour laquelle il a été entraîné ; Simon Willison a refait l'expérience avec un modèle plus petit. Jessica Kerr trouve plus agréable de se faire enseigner des choses par l'IA que de lui en enseigner. Houck et al. proposent CAFE(S), une grille pour juger le contexte fourni à un agent (clarté, actionnabilité, fidélité, efficacité, sécurité), avec six « context smells », c'est-à-dire des signes de mauvais contexte. Fowler en trouve l'approche utile, mais pas le nom ni le style.

Enfin, Cindy Sridharan indique qu'elle consacre environ 70 % de son budget quotidien de tokens à vérifier des patchs et seulement 20 à 30 % à les produire. Fowler fait le parallèle avec les entreprises, qui dépensent davantage de tokens en garde-fous et en évaluations qu'en génération de réponses.
