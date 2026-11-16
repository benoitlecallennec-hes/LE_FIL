---
title: "Agents au long cours : l’exécution durable pour survivre aux pannes"
date: 2026-11-10T14:10:00.000Z
source: martinfowler.com
link: https://martinfowler.com/articles/durable-execution-agents.html
tags:
  - autres
lang: en
level: intermediate
format: article
reading_time: 18
image: https://martinfowler.com/articles/durable-execution-agents/card.png
summary: Sur martinfowler.com, Kiran Prakash montre comment l’exécution durable permet à un agent qui travaille pendant des jours ou des semaines de survivre aux plantages et aux longues attentes, en rejouant un journal des étapes déjà faites. Il explique aussi pourquoi cela ne suffit pas à rendre l’agent correct.
takeaways:
  - L’exécution durable enregistre le résultat de chaque étape externe. Après une panne, le programme est rejoué, saute les étapes faites et reconstruit son état.
  - "Les attentes doivent elles aussi être durables (`workflow.wait_condition` avec `timeout`) : un agent inactif ne garde ni processus ni mémoire."
  - Les reprises donnent un fonctionnement au moins une fois, donc chaque écriture doit porter une clé d’idempotence.
  - "La durabilité ne garantit pas la justesse : il faut des actions compensatoires, des revérifications avant d’agir, des règles versionnées et une méfiance envers le texte extérieur."
status: published
featured: false
edition: 2026-W47
---
Kiran Prakash, de Thoughtworks, part d’un exemple suivi tout au long de l’article : Stock Replenisher, un agent qui réapprovisionne en pièces un industriel. La boucle agentique est faite d’un objectif, d’outils, d’un état et d’un modèle. Elle peut durer des semaines et échanger avec des fournisseurs et des humains. Pendant ce temps, beaucoup de choses peuvent mal tourner : le processus plante, un appel relancé passe une commande en double, une décision ne vaut plus quand vient le moment de l’appliquer.

L’exécution durable repose sur trois idées : des étapes, un journal et un rejeu. Le runtime enregistre le résultat de chaque étape qui touche le monde extérieur. Après une panne, le programme repart du début, saute les étapes déjà faites et reconstruit son état à partir du journal. L’auteur rend la boucle durable avec les primitives de Temporal : `workflow.execute_activity` pour les appels au modèle et aux outils, `workflow.wait_condition` avec un `timeout` pour les attentes. Pendant ces attentes durables, un agent inactif n’occupe ni processus ni mémoire. Il sépare les workflows d’entité (un par référence produit) des workflows de processus (un par réapprovisionnement). Comme les reprises donnent un fonctionnement au moins une fois (at-least-once), les écritures exigent des clés d’idempotence. Un encadré compare Temporal et Cloudflare Workflows.

Selon l’auteur, être durable ne veut pas dire être correct. Certains problèmes demandent des parades métier : des actions compensatoires comme `cancel_order`, des approbations qui expirent, ou une fourchette de prix approuvée plutôt qu’un montant exact. L’agent revérifie aussi les conditions juste avant d’engager une dépense :

```python
class PlaceOrder(Call):
    ...
    async def execute(self, state, context):
        quotes = await context.supplier.get_quotes(state.sku)
        price = ...  # this supplier's price today, from quotes
        if price > state.approved_up_to:  # above the range: don't order
            return PlaceOrderResult(placed=False, price=price)
        ...  # within the range: order at today's price
```

Pour changer une règle métier pendant que des workflows tournent encore, il faut versionner (par exemple avec un champ `rules_version` dans l’état) et procéder par changement parallèle. La mémoire qui grossit est un autre problème, tout comme le texte extérieur qui peut orienter l’agent : c’est au harnais, pas au runtime, de choisir ce que voit le modèle et de traiter ce texte comme non fiable. En conclusion, l’auteur rappelle ce qu’il faut pour qu’un agent soit utile en entreprise : l’expertise du domaine, l’architecture logicielle, l’intégration aux systèmes historiques et des data products.
