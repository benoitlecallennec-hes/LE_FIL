---
title: "Unity Vector : relier les données MMP, Google Analytics et achats intégrés pour mieux cibler les joueurs"
date: 2026-11-24T00:00:00.000Z
source: Unity Blog
link: https://unity.com/blog/data-fragmentation-ua-monetization-unity-vector
tags:
  - 3d
lang: en
level: beginner
format: article
reading_time: 4
summary: Unity explique que les équipes d'acquisition d'utilisateurs (UA) et de monétisation travaillent avec des données dispersées entre plusieurs plateformes. L'article détaille les signaux à connecter à Unity Vector, le moteur d'optimisation par IA de Unity Ads, pour qu'il repère les joueurs les plus rentables.
takeaways:
  - Le MMP indique d'où vient une installation. Google Analytics explique ce que fait le joueur ensuite. Les deux se complètent.
  - Unity Vector, le moteur d'IA de Unity Ads, optimise mieux quand on lui connecte installations, sessions, achats, revenus publicitaires et IAP.
  - "Configuration recommandée : postback « Lifetime », envoi des événements organiques et non attribués, Google Analytics lié au Unity Dashboard, SDK Unity Ads et IAP à jour."
  - Envoyer des événements non attribués suppose d'avoir les consentements et les droits nécessaires.
missing:
  - image principale (balise og:image non lue)
status: published
featured: false
edition: 2026-W48
---
Selon Unity, les équipes d'acquisition d'utilisateurs (UA) et de monétisation ne manquent pas de données. Elles manquent d'une vue d'ensemble. Les dépenses, les installations, le comportement en jeu, les revenus publicitaires et les achats intégrés (IAP) sont répartis entre des plateformes séparées. Cette dispersion fausse les décisions sur les enchères, les budgets et les créations publicitaires. L'article en décrit trois coûts. D'abord, l'optimiseur d'une plateforme publicitaire ne voit qu'une partie de la valeur d'un joueur. Ensuite, le parcours du joueur perd son contexte, alors qu'une conversion dépend rarement d'un seul point de contact. Enfin, chaque outil donne sa propre version des événements, et l'on ne sait plus laquelle est la plus complète.

L'article compare deux sources principales. Le partenaire de mesure mobile (MMP) indique d'où vient une installation (campagne, réseau, création), ce qu'elle a coûté et ce qu'elle rapporte. Il ne couvre toutefois que les événements que le studio a configurés. Il ne montre donc ni le niveau où un joueur reste bloqué, ni le parcours qui précède son départ, ni la fonctionnalité qui précède un achat. Google Analytics enregistre beaucoup plus d'événements en jeu et suit le parcours après l'entrée dans l'application. Il explique pourquoi un joueur rentable a converti. Il complète l'attribution du MMP mais ne la remplace pas.

Unity Vector apprend à partir de ces signaux connectés : installations, sessions, achats, revenus publicitaires, rétention et ROAS. Ses prédictions s'améliorent à mesure qu'on lui en fournit davantage. La liste de configuration recommandée couvre trois zones. Dans le MMP, il faut activer l'indicateur d'installations organiques et envoyer aussi les événements non attribués. Il faut aussi choisir une fenêtre de postback « Lifetime » et transmettre installations, sessions, achats et revenus publicitaires. Dans le Unity Dashboard, il faut lier Google Analytics, fournir la clé de licence Android et configurer le framework Developer Data. Côté SDK, il faut tenir à jour le Unity Ads SDK et le Unity IAP SDK, et configurer les diagnostics dans l'éditeur Unity. Avant d'envoyer des événements non attribués, le studio doit disposer des avis, des consentements et des droits nécessaires au partage de ces données.

L'article cite trois studios, sans donner de chiffres. Chez Dobest, de meilleurs postbacks ont permis à Vector d'exploiter les installations, la rétention, les revenus publicitaires et IAP et le ROAS. Selon le studio, sa campagne optimisée sur le ROAS à 28 jours est devenue plus stable à moyen et long terme, avec des utilisateurs de meilleure qualité et un meilleur ROAS. Wonder Kingdom a intégré dans Vector l'attribution du MMP, ses données de monétisation et le comportement en jeu mesuré par Google Analytics. Kooapps témoigne sur la différence entre des données fragmentées et des sources multiples bien reliées.
