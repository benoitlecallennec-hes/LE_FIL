---
title: "Disponibilité de GitHub en octobre 2026 : migration vers Azure, pannes d'écriture et tempêtes de reconnexion"
date: 2026-11-13T00:20:16.000Z
source: The GitHub Blog
link: https://github.blog/news-insights/company-news/github-availability-report-october-2026/
tags:
  - autres
feed_tag: News & insights
lang: en
level: intermediate
format: article
reading_time: 15
image: https://github.blog/wp-content/uploads/2026/11/oct5-actions-jobs-starting-late.png?resize=1024%2C466
summary: Le rapport mensuel de GitHub fait le point sur la migration vers Azure Central US, désormais bien avancée. Il détaille aussi sept incidents d'octobre, dont deux pannes d'écriture causées par un routage de base de données qui envoyait les écritures vers des serveurs retirés.
takeaways:
  - Au 9 novembre, 140 des 151 services de production et 32 des 50 primaires MySQL de GitHub tournaient dans Azure Central US.
  - Les pannes d'écriture du 7 octobre venaient d'une machine de routage partie d'enregistrements périmés, qui envoyait les écritures vers des serveurs retirés.
  - Attendre l'expiration d'un appel au lieu d'échouer vite, ou se reconnecter tous en même temps, a provoqué plusieurs incidents. GitHub prévoit des délais aléatoires avant les reconnexions et des limites sur les nouvelles connexions.
  - Garder une connexion de pool pendant une recherche lente a suffi à épuiser le pool partagé et à faire échouer 43 % des fusions de pull requests.
missing:
  - balise og:image non lue (image d'en-tête visible utilisée à la place)
status: rejected
draft: true
featured: false
---
Au 9 novembre, 140 des 151 services de production et 32 des 50 primaires MySQL de GitHub tournaient dans Azure Central US. Azure servait 63 % des lectures des services migrés, 66 % de celles du monolithe et 52 % des lectures Git. La capacité des neuf clusters Kubernetes de la région a plus que doublé, et le trafic des services migrés a augmenté de 82 % depuis mars. Plusieurs corrections ciblées ont réduit la charge. Un endpoint de GitHub Actions qui reçoit plus de 250 millions de requêtes par jour passe désormais par un chemin plus efficace, ce qui divise par sept environ les lectures de base de données en pointe (−85 %). Imposer au moins trois caractères avant de chercher dans les topics de dépôts a supprimé environ 98 % des requêtes lentes ou interrompues pour ce motif. Une mitigation sur la passerelle NAT a réduit son usage de 85 %. Le chantier East US 3 a environ une semaine de retard, et sa première montée de trafic validée est prévue début décembre.

Les deux pannes d'écriture du 7 octobre, d'une dizaine de minutes chacune, viennent d'une interaction dangereuse entre l'orchestration des bases et le routage de production. Pendant une maintenance, une machine de remplacement chargée du routage des écritures a démarré avec des enregistrements périmés qui listaient des serveurs retirés. Elle est devenue leader et a dirigé les écritures de plusieurs bases centrales vers ces serveurs. Une tâche de maintenance automatique a ensuite ignoré la pause demandée et relancé une élection, ce qui a provoqué la seconde panne. Jusqu'à 9,3 % des requêtes web et API ont échoué. Aucune donnée enregistrée n'a été perdue.

Plusieurs autres incidents relèvent de la même famille : des appels qui attendent au lieu d'échouer vite, ou des reconnexions simultanées. Le 5 octobre, le service qui attribue les jobs Actions aux runners hébergés attendait l'expiration d'appels réseau vers ses bases. Ces appels se sont accumulés jusqu'à épuiser la mémoire, puis la charge a basculé vers une région sans capacité suffisante. Le 19 octobre, tous les serveurs d'un service Actions ont relancé en même temps leurs connexions vers un cache, et ces tentatives ont saturé le lien entre le datacenter et la région cloud. Le 21 octobre, des reconnexions immédiates ont fait grimper la mémoire de la plateforme de streaming d'événements. Les journaux d'audit ont mis environ 6 h 46 à rattraper leur retard. GitHub prévoit des délais aléatoires avant chaque reconnexion et des limites sur l'ouverture de nouvelles connexions.

Le 28 octobre, un identifiant devenu invalide auprès d'un fournisseur de modèles a fait échouer environ une requête sur dix vers plusieurs modèles GPT dans Copilot. Le 30 octobre, le pool de connexions de base de données partagé par de nombreux services de github.com a atteint sa limite. Des requêtes de mise à jour gardaient leur connexion pendant une recherche lente qui n'en avait pas besoin. Au pire moment, environ 43 % des fusions de pull requests ont échoué. Le rapport rappelle la règle que GitHub s'est fixée : la disponibilité d'abord, puis la capacité, puis les fonctionnalités.
