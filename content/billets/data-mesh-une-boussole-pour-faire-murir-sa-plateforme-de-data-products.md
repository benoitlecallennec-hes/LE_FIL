---
title: "Data mesh : une boussole pour faire mûrir sa plateforme de data products"
date: 2026-11-05T14:11:00.000Z
source: martinfowler.com
link: https://martinfowler.com/articles/data-product-platform-nav.html
tags:
  - autres
lang: en
level: intermediate
format: article
reading_time: 18
image: https://martinfowler.com/articles/data-product-platform-nav/card.png
summary: Sur martinfowler.com, Tom De Wolf propose un « navigateur » fondé sur le modèle de maturité Platform Engineering de la CNCF. Il aide les équipes data mesh à situer leur plateforme self-service et à choisir où investir ensuite.
takeaways:
  - Le modèle de la CNCF compte quatre niveaux (Provisional, Operational, Scalable, Optimizing) évalués sur cinq axes.
  - Le niveau Scalable, avec une plateforme gérée comme un produit et un portail self-service, est le minimum pour un data mesh qui fonctionne.
  - "Automatiser l'infrastructure ne suffit pas : l'adoption passe par l'expérience développeur, l'évangélisation et la formation."
  - "Investir de façon ciblée : traiter en priorité l'aspect qui crée le plus de friction, puis recommencer."
status: rejected
draft: true
featured: false
---
Beaucoup d'organisations qui adoptent le data mesh doivent construire une plateforme self-service pour leurs data products. D'autres en ont déjà une, mais elle est peu adoptée et empêche le data mesh de passer à l'échelle. Tom De Wolf (ACA Group) part d'un constat : aucune technologie ne couvre à elle seule un data product complet. Il reste donc un écart entre les concepts du data mesh et l'outillage. Pour le combler, il propose une couche de platform engineering qui élève le niveau d'abstraction et réduit la charge cognitive des équipes métier.

Pour se repérer, l'auteur reprend les quatre niveaux du modèle de maturité de la CNCF. Au niveau Provisional, les demandes passent de personne à personne et l'infrastructure est assemblée à la main. Au niveau Operational, les demandes sont standardisées par tickets ou par scripts d'infrastructure as code (`createGitRepository`, `createCIPipeline`, `createS3Bucket`, `createIcebergDatabase`, `createSparkJob`). Les développeurs doivent pourtant encore relier ces briques eux-mêmes, ce qui donne des produits « Frankenstein » et du shadow IT. Au niveau Scalable, la plateforme est gérée comme un produit : un portail self-service provisionne et câble tous les composants. Au niveau Optimizing, elle devient un écosystème intégré aux IDE et aux CLI, auquel les équipes contribuent et dont les données d'usage guident les évolutions. Chaque niveau est évalué sur cinq axes : investissement, adoption, interfaces, opérations et mesure.

Selon l'auteur, le niveau Scalable est le minimum pour réussir un data mesh, car c'est à partir de là que l'adoption peut devenir volontaire. Il met en garde contre un piège : automatiser la technique en négligeant l'expérience développeur. Il recommande d'équilibrer personnes, processus et technologie, et de créer une équipe produit dédiée avant d'ajouter des fonctionnalités. La méthode consiste à cartographier l'état actuel sur le modèle, à repérer l'aspect qui crée le plus de friction et à y concentrer le prochain cycle d'investissement. Le modèle sert aussi à dialoguer avec les parties prenantes.
