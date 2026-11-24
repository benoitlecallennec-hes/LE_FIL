---
title: "Les contrats de données : décrire un jeu de données comme on décrit une API"
date: 2026-11-18T14:02:00.000Z
source: martinfowler.com
link: https://martinfowler.com/articles/data-contracts.html
tags:
  - autres
lang: en
level: intermediate
format: article
image: https://martinfowler.com/articles/data-contracts/social-card.png
summary: Simon Harrer et Arif Wider appliquent aux données les pratiques de conception d'API. Un contrat de données est un document lisible par machine qui décrit le contenu et les garanties d'un jeu de données. Ils s'appuient sur l'Open Data Contract Standard (ODCS), dont la version `v3.2.0` ajoute un contexte pour l'IA.
takeaways:
  - Un contrat de données décrit en YAML le schéma, la qualité, les SLA, la sécurité et les conditions d'utilisation d'un jeu de données.
  - ODCS, porté par Bitol (Linux Foundation), en est à la version 3.2, qui ajoute le contexte pour l'IA. Un schéma JSON permet de valider les contrats.
  - Le contrat est publié par le fournisseur seul. L'accès passe par un accord bilatéral que le fournisseur doit approuver.
  - L'outil Data Contract CLI prend en charge ODCS 3.1.
missing:
  - dernières sections de l'article (mise en œuvre, coûts et bénéfices)
  - durée de lecture
  - image principale
status: selected
draft: true
featured: false
edition: 2026-W48
---
Un ingénieur de données doit savoir si un jeu de données lui convient, s'il est de bonne qualité, s'il restera stable et ce qu'il a le droit d'en faire. Le contrat de données répond à ces questions en continu, et non une seule fois. Dans sa forme la plus simple, c'est la description de la forme des données qu'un fournisseur propose. Les auteurs lui donnent six caractéristiques : il est lisible par machine, vérifiable, versionné, porté par le fournisseur, orienté vers le consommateur, et il sert de source de vérité pour les métadonnées. Les données sont ainsi traitées comme des services, par exemple des produits de données, dont l'interface doit être définie aussi clairement que possible.

L'article passe en revue les sections de l'Open Data Contract Standard (ODCS) :
- les fondamentaux ;
- le schéma et la qualité des données ;
- la propriété et le support ;
- les conditions d'utilisation et les SLA ;
- l'emplacement des serveurs (Snowflake, Databricks, BigQuery, Postgres, Kafka, S3…) ;
- les rôles et la sécurité ;
- le contexte pour l'IA et la tarification ;
- l'extensibilité.

Un contrat commence par un bloc d'identification :

```yaml
apiVersion: "v3.2.0"
kind: "DataContract"
id: "customers"
name: "Customers"
version: "1.0.0"
status: "active"
```

ODCS vient du « Data Contract Template » de PayPal, publié en open source en 2023. Il est passé par une version 2.2, devenue la norme fondatrice du projet Bitol de la Linux Foundation. La version 3.0 (2024) a retiré les champs propres à PayPal, la 3.1 (2025) a remplacé la Data Contract Specification et la 3.2 (2026) a ajouté le contexte pour l'IA. Le standard fournit un schéma JSON qui permet de valider les contrats. L'outil Data Contract CLI prend en charge ODCS 3.1 nativement.

Les auteurs replacent ensuite le contrat dans un ensemble plus large. Il peut être relié à une ontologie. Il définit les ports d'entrée et de sortie d'un produit de données, qui s'appuie sur l'Open Data Product Standard (ODPS). Il sert aussi de « fiche produit » dans une place de marché de données. Un contrat est publié par le fournisseur seul. L'accord d'accès, lui, est bilatéral : il réunit le contrat, la finalité du traitement déclarée et le consommateur qui demande l'accès. Il ne prend effet qu'une fois approuvé par le fournisseur, et un agent d'IA peut n'obtenir l'accès que pour quelques minutes. Selon les auteurs, cette démarche rend les grands ensembles de données plus robustes. Elle donne aussi aux humains comme aux agents d'IA les métadonnées explicites et fiables dont ils ont besoin pour travailler seuls avec les données.
