---
title: Les tests par simulation déterministe, pour rendre rejouables les pannes des systèmes distribués
date: 2026-11-17T14:33:00.000Z
source: martinfowler.com
link: https://martinfowler.com/articles/deterministic-simulation-testing.html
tags:
  - autres
lang: en
level: advanced
format: article
summary: Unmesh Joshi explique comment remplacer l'horloge, le réseau et le disque par des composants simulés, pilotés par une boucle à un seul thread qui n'avance qu'à chaque appel de `tick()`. Avec la même graine, un système distribué exécute alors exactement le même scénario de panne à chaque lancement.
takeaways:
  - "Une seule boucle sur un seul thread, avec une horloge, un réseau et un disque simulés : la même graine donne la même exécution"
  - Les composants implémentent `Tickable` et n'avancent qu'à chaque appel de `tick()` ; les timeouts se comptent en ticks
  - "`SimulatedNetwork` remplace `NioNetwork` derrière l'interface `Network` et peut couper le réseau en partitions, perdre ou retarder les messages, lien par lien"
  - Simuler au niveau des messages (TigerBeetle) n'exerce pas le code de découpage du flux, contrairement à une simulation au niveau des octets (FoundationDB)
missing:
  - seconde moitié de l'article lue seulement via un résumé automatique, sans texte exact (boucle de simulation, stockage simulé, scénarios du magasin à quorum, vérification de cohérence)
  - image principale
  - durée de lecture exacte
status: selected
draft: true
featured: false
edition: 2026-W48
---
Les systèmes distribués échouent de façons que les tests classiques et le chaos engineering en production ne savent pas reproduire. Le chaos engineering vérifie surtout que le système reste disponible, ce qui ne prouve pas qu'il soit correct. Les vérifications à la Jepsen détectent bien des violations de cohérence, mais une panne observée en conditions réelles se rejoue mal. Deux sources de non-déterminisme sont en cause : des pannes injectées sans graine, et le timing d'exécution (horloges, ordonnancement des threads, livraison des messages, écriture sur disque). Le test par simulation déterministe les supprime. Tout le cluster tourne sur un seul thread, face à une horloge, un réseau et un disque simulés, et rien ne progresse tant que le test n'appelle pas `tick()`.

Chaque processus suit le motif Singular Update Queue. Les appels réseau et stockage sont asynchrones, aucun ne bloque, et chaque composant implémente l'interface `Tickable`. Le code du protocole ne voit qu'une interface `Network`. En test, `SimulatedNetwork` transmet les messages en mémoire et peut, lien par lien, couper le réseau en partitions, perdre des messages ou les retarder. En production, `NioNetwork` passe par de vrais sockets. `send()` met un message en file avec son tick de livraison, et `tick()` fait avancer la simulation :

```java
  @Override
  public void tick() {
      currentTick++; // Increment internal counter (TigerBeetle pattern)
      maybeFlipPartitions();
      maybeClogPaths();
      deliverPendingMessagesFor(currentTick);
  }
```

Les délais d'expiration se comptent en ticks, pas en temps réel : un timeout de 10 expire au bout d'exactement 10 appels à `tick()`, comme dans etcd-raft. L'article compare aussi deux niveaux de simulation. TigerBeetle simule au niveau des messages, ce qui laisse de côté le code de découpage du flux en messages (framing). FoundationDB simule au niveau du flux d'octets.

L'exemple suivi tout au long de l'article est un magasin clé-valeur à quorum : trois réplicas, et un coordinateur qui confirme une écriture dès qu'une majorité l'a acceptée. Le code vient de Tickloom, une implémentation pédagogique en Java inspirée de FoundationDB et TigerBeetle. Elle simule le cluster, le réseau et les fichiers, avec un lanceur à graine et un vérificateur de cohérence.
