---
title: IBM et Confluent branchent les modèles de séries temporelles Granite directement dans le flux de données
date: 2026-11-18T15:00:23.000Z
source: Hugging Face Blog
link: https://huggingface.co/blog/ibm-research/time-series-powered-streamhouse
tags:
  - ia
lang: en
level: intermediate
format: article
reading_time: 11
image: https://cdn-uploads.huggingface.co/production/uploads/64e8143f6de557454220921e/uNQkq4JD8ABh729KrjG0A.webp
summary: IBM Research et Confluent intègrent les modèles de séries temporelles Granite dans Apache Flink, appelés depuis SQL via `AI_FORECAST` et `AI_DETECT_ANOMALIES`. Les prévisions et la détection d'anomalies se font ainsi sur les données en mouvement, et non plus en lot après coup.
takeaways:
  - Les modèles Granite (TTM, PatchTST-FM, TSPulse) sont appelés depuis Flink SQL via `AI_FORECAST` et `AI_DETECT_ANOMALIES`
  - Un modèle pré-entraîné couvre de nombreuses séries sans entraînement dédié à chacune, et il tourne sur CPU
  - Les prévisions sont données avec une fourchette P10–P90, ce qui permet de raisonner en plages et non en valeurs uniques
  - "Six cas d'usage : production, commerce, chaîne logistique, fraude, trading et recherche de motifs similaires"
missing:
  - image principale (og:image) non exposée
status: rejected
draft: true
featured: false
---
L'article d'IBM Research part d'un constat : une prévision ou une alerte d'anomalie n'a de valeur que si elle arrive pendant que les données circulent encore, assez tôt pour changer le résultat. Les modèles de fondation pour séries temporelles doivent donc tourner dans le pipeline de streaming lui-même. Le cadre retenu est le « Streamhouse » de Confluent, une façon de capter, traiter, gouverner et servir en continu le contexte métier, à destination des applications, de l'analytique et des agents. Apache Flink y prépare les données, appelle les fonctions de prévision et de détection, puis place les résultats à côté des événements source. Les Lightning Tables donnent un accès à faible latence à l'état courant.

Le pilotage se fait en SQL. `AI_DETECT_ANOMALIES` donne à chaque série une plage attendue à un niveau de confiance choisi. `AI_FORECAST` produit des prévisions avec une fourchette P10–P90. Derrière ces fonctions se trouve la famille Granite (quatre modèles, plus de 50 millions de téléchargements), dont TTM, PatchTST-FM et TSPulse. L'argument central : un modèle pré-entraîné sert de point de départ commun à de nombreuses séries, sans entraîner un modèle par série. Il reste possible de l'affiner ou de combiner ses sorties avec d'autres données et des règles métier. Les modèles tournent sur CPU, sans GPU. Le service est entièrement géré dans Confluent Cloud ; sur site, il passe par Confluent Platform for Apache Flink.

Six cas d'usage illustrent l'approche. En production, un modèle TTM lit toutes les dix minutes la télémétrie SCADA et les mesures qualité d'une ligne de glaces, et prévoit à quatre heures la part de produits conformes. Si la prévision passe sous l'objectif, un optimiseur teste des consignes que valide un opérateur. En commerce de détail, la détection d'anomalies repère les baisses de ventes discrètes que des seuils fixes laissent passer. En logistique, une prévision à 28 jours par produit et par magasin alimente le calcul du point de commande, puis un agent rédige un bon de commande. Pour la fraude, chaque paiement est comparé à la plage tirée de l'historique de la carte. En trading, PatchTST-FM prévoit le volume des 30 prochaines minutes pour cadencer un ordre de 100 000 titres. Enfin, les embeddings de TSPulse servent à retrouver des motifs similaires d'un équipement à l'autre, sans pour autant établir une cause racine.
