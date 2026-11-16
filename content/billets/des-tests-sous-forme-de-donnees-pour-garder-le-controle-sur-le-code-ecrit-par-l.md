---
title: Des tests sous forme de données pour garder le contrôle sur le code écrit par l'IA
date: 2026-11-11T15:17:00.000Z
source: martinfowler.com
link: https://martinfowler.com/articles/tests-as-data.html
tags:
  - autres
lang: en
level: intermediate
format: article
reading_time: 27
image: https://martinfowler.com/articles/tests-as-data/card.png
summary: "Matteo Vaccari (Thoughtworks) propose de sortir le contrat entre développeurs et agents IA du code de test pour le placer dans des données lisibles : tableaux, fichiers texte et petits DSL. Les agents ont alors plus de mal à l'affaiblir sans que personne le remarque."
takeaways:
  - "Quand l'IA écrit le code, le développeur en devient le client : ce sont les tests client en boîte noire qui servent de contrat."
  - Placer ce contrat dans des données (tableaux, YAML, petits DSL) plutôt que dans du code le rend lisible et plus difficile à affaiblir en douce.
  - L'IA rend bon marché les analyseurs de DSL et les lanceurs de tests sur mesure, qui coûtaient autrefois cher.
  - Il faut protéger les fichiers de test (lecture seule, contrôle en CI) et vérifier les tests par mutation testing et oracles.
status: published
featured: false
edition: 2026-W47
---
Quand des agents IA écrivent à la fois le code de production et les tests, les développeurs risquent de ne plus pouvoir s'y fier. L'auteur s'appuie sur une étude de 2026 menée auprès de 26 développeurs : ceux-ci ne faisaient pas confiance aux tests maintenus par l'IA, parce que l'agent refactorait sans cesse le code et les tests en même temps. Il relève aussi plusieurs défauts des tests générés : ils sont trop nombreux, peu lisibles, liés à l'implémentation, et ils valident parfois des bugs existants. Les spécifications rédigées en prose ont leurs propres limites : elles sont longues, difficiles à maintenir, ambiguës et rarement respectées en entier.

Sa thèse tient en trois points. Quand l'IA écrit le code, le développeur en devient le client. Les tests client (en boîte noire, centrés sur le comportement et écrits dans la langue du métier) remplacent donc les tests développeur comme contrat principal. Enfin, ce contrat est mieux protégé s'il est exprimé en données plutôt qu'en code. Concrètement, on ne garde que le nom du test, l'entrée et la sortie attendue. Un test HTTP Java devient ainsi un fichier YAML, puis un tableau de type CSV. Un scénario Gherkin sur les droits d'administration devient un tableau qui croise rôles, états de session et ressources, ce qui rend la couverture et ses trous visibles.

```yaml
suite: greetings
tests:
  - name: "default greeting"
    request:
      resource: "/hello"
      name: null
    greeting: "Hello, world!"
  - name: "personalised greeting"
    request:
      resource: "/hello"
      name: "Joe"
    greeting: "Hello, Joe!"
```

Pour les entrées complexes, l'auteur recourt à des DSL ad hoc. Ses exemples : l'état d'un éditeur XML avant et après une opération, avec un marqueur de curseur ; une suite de tests YAML pour un interpréteur BASIC ; un DSL `GIVEN`/`WHEN`/`THEN` pour une todo-list, avec un convertisseur qui transforme le HTML en texte comparable par diff ; un DSL pour une liste blanche d'adresses IP. Il rattache cette démarche à plusieurs traditions : Fit de Ward Cunningham et la spécification par l'exemple, les tests en tableaux et les scripts de test de Go, l'approval testing, et les Approved Scenarios d'Ivett Ördög. Écrire des analyseurs de DSL et des lanceurs de tests sur mesure coûtait autrefois cher ; l'IA rend aujourd'hui ces outils bon marché.

Côté mise en œuvre, les tests n'ont pas tous besoin d'être de bout en bout. Pour les systèmes externes, on peut utiliser des simulateurs (WireMock, Mountebank ou des « jumeaux numériques » générés par IA). Les données de test doivent être protégées : en lecture seule, ou contrôlées par la CI. On vérifie la valeur des tests par mutation testing et avec des oracles : documentation officielle, implémentation de référence par force brute, ou exécution sur le système réel. L'auteur recommande d'y ajouter des tests de propriétés, des linters et des tests exploratoires manuels. Il juge l'approche adaptée aux systèmes en production, mais excessive pour un prototype.
