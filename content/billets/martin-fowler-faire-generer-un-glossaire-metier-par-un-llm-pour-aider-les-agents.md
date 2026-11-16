---
title: "Martin Fowler : faire générer un glossaire métier par un LLM pour aider les agents"
date: 2026-11-12T14:15:00.000Z
source: martinfowler.com
link: https://martinfowler.com/bliki/GeneratedGlossary.html
tags:
  - autres
feed_tag: bliki
lang: en
level: intermediate
format: article
reading_time: 5
summary: "Dans son bliki, Martin Fowler décrit une technique simple : faire extraire par un LLM le vocabulaire propre à une organisation, le faire relire par des experts, puis le mettre à disposition des autres LLM qui travaillent dans ce domaine."
takeaways:
  - Un LLM peut extraire le vocabulaire propre à une organisation, y compris les termes connus employés dans un sens inhabituel.
  - Des experts du domaine doivent relire le glossaire, qui est ensuite versionné et révisé régulièrement.
  - Il vaut mieux un glossaire par contexte délimité (au sens du DDD) qu'un glossaire unique et normalisé.
  - Le glossaire guide le nommage dans le code et l'interface, et peut signaler les termes à éviter.
status: published
featured: false
edition: 2026-W47
---
Quand on utilise des LLM dans un domaine spécialisé, comme le fonctionnement interne d'une grande organisation, on se heurte à un vocabulaire qui leur est étranger. Ces termes, ou ces sens particuliers, ne figurent pas dans leurs données d'entraînement. Martin Fowler propose de demander à un LLM de produire un glossaire à partir des documents internes : documents, e-mails, historiques de discussion, voire transcriptions audio ou vidéo. Le générateur doit repérer les mots isolés et les expressions de plusieurs mots qui forment un terme. Il doit aussi relever les termes connus employés dans un sens inhabituel, souvent les plus trompeurs. Chaque terme reçoit une définition précise et concise, dans le format d'un dictionnaire. Si un terme a plusieurs sens, chacun est donné avec le contexte où il apparaît.

Des experts du domaine relisent ensuite ce brouillon et corrigent les contresens. Le glossaire est rangé là où les autres LLM peuvent le consulter facilement, sous une forme qui leur coûte peu de tokens. Il est conservé sous gestion de versions et réexaminé régulièrement, par des agents comme par des humains. Si le glossaire est volumineux, on peut ne placer dans le contexte initial que les termes et des liens vers leurs définitions. Le modèle ne consulte alors que celles dont il a besoin.

Fowler s'appuie sur la notion de contexte délimité (bounded context) du Domain-Driven Design. Les grandes organisations ont souvent des définitions légèrement différentes selon les services, et le code source peut lui-même former un contexte à part. Selon les cas, il vaut mieux un glossaire couvrant plusieurs contextes ou un glossaire par contexte. Fowler déconseille en revanche de viser un glossaire unique et normalisé, qu'il n'a jamais vu fonctionner. Pour lui, l'intérêt d'un glossaire est souvent de montrer comment l'usage d'un terme varie d'un contexte à l'autre. Comparer plusieurs glossaires, par exemple celui tiré du code et celui tiré des textes humains, peut être très instructif.

En ingénierie agentique, le glossaire aide à générer du code dont les noms suivent le vocabulaire de l'entreprise. Des capteurs placés dans le harnais de l'agent peuvent vérifier cet usage, dans le code comme dans l'interface utilisateur. Le glossaire peut aussi signaler les termes à éviter, comme le jargon interne qui ne doit pas apparaître dans une interface destinée aux clients, et proposer des remplacements. Fowler y voit enfin un premier pas concret vers un travail plus large de modélisation du domaine et d'ontologie.
