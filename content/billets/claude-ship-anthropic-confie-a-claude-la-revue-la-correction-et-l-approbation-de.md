---
title: "Claude Ship : Anthropic confie à Claude la revue, la correction et l'approbation des pull requests"
date: 2026-11-23T00:00:00.000Z
source: claude.com
link: https://claude.com/fr/resources/articles/introducing-claude-ship
tags:
  - ia
lang: en
level: beginner
format: article
reading_time: 6
summary: Anthropic lance Claude Ship, qui relit les pull requests GitHub, les corrige et approuve celles qu'autorise la politique de l'équipe. Côté développeur, tout part d'une nouvelle commande `/ship` dans Claude Code.
takeaways:
  - Avec `/ship` dans Claude Code, Claude pousse le changement, ouvre la PR, la relit et traite les remarques et les tests en échec.
  - PR Steward corrige la CI, les conflits et les remarques, mais n'approuve ni ne fusionne jamais. On l'arrête avec `@claude stop`.
  - Claude Approvals applique une politique d'approbation versionnée dans le dépôt, à la manière d'un `CODEOWNERS` enrichi.
  - Le service est réservé aux offres Team et Enterprise sur GitHub et facturé aux tokens. Code Review est disponible pour tous, PR Steward et Claude Approvals sont en bêta.
missing:
  - image principale (og:image)
  - exemples de fichiers de politique, visibles seulement en images
  - auteur
status: selected
draft: true
featured: false
edition: 2026-W48
---
Selon Anthropic, attendre une revue, revenir sur du code ancien, pousser des corrections puis attendre une nouvelle revue coupe le développeur dans son travail. L'approbation humaine de chaque changement reste gérable quand ce sont des personnes qui écrivent le code. Elle devient plus difficile quand des agents en écrivent une grande partie. Dans l'équipe Claude Code, c'est Claude qui écrit la majorité du code. Claude Ship, en bêta, relit les pull requests, les corrige et décide ce qui peut être fusionné, selon des règles que chaque équipe fixe.

Le produit réunit trois briques. Code Review, déjà disponible pour tous, analyse une PR sous plusieurs angles en tenant compte du code existant. Il vérifie ses propres constats de manière contradictoire, les classe et les publie sur la PR, avec trois niveaux de revue (Quick, Standard, Deep). PR Steward, en bêta, garde la PR au vert : il réagit aux échecs de CI, aux conflits de fusion et aux remarques des relecteurs. Après chaque push, il indique ce qu'il a modifié et comment l'annuler, mais il n'approuve ni ne fusionne jamais. On le pilote en commentant `@claude` suivi d'une consigne, ou `@claude stop` pour qu'il s'arrête. Claude Approvals, en bêta lui aussi, applique une politique d'approbation écrite dans un fichier du dépôt, présentée comme « un fichier `CODEOWNERS` plus intelligent ». Cette politique peut s'appuyer sur les constats de Code Review ou sur des classificateurs rédigés en langage naturel.

Il y a deux façons de démarrer. Un développeur lance `/ship` dans Claude Code : Claude pousse le changement, ouvre la PR, la relit, puis traite les remarques et les tests en échec. Un administrateur peut aussi activer le service sur un dépôt pour qu'il couvre toutes les PR de l'équipe. Dans ce cas, il choisit le niveau de revue, le moment où les revues se lancent et un plafond de dépenses. Claude Ship s'intègre aux protections de branche et aux vérifications obligatoires déjà en place. Anthropic conseille de faire d'abord tourner Claude Approvals en simple observation, avant d'exiger son feu vert. Selon l'article, dans l'équipe Claude Code, le nombre de PR fusionnées a plus que doublé entre juin et septembre, et la part de PR annulées a baissé de plus de 40 %.

Le service est proposé sur les offres Team et Enterprise, pour les dépôts GitHub. Il n'est pas accessible aux organisations en Zero Data Retention, en configuration HIPAA ou avec des clés de chiffrement gérées par le client. La facturation se fait aux tokens consommés, sauf pour Claude Approvals, qui n'a pas de coût de base. Lancer `/ship` compte comme un usage ordinaire de Claude Code.
