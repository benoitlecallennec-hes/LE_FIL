---
title: Pourquoi un fine-tuning dure plus longtemps que ne le prédit le calcul des FLOPs
date: 2026-11-18T18:53:43.000Z
source: Hugging Face Blog
link: https://huggingface.co/blog/Dharma-AI/predicting-fine-tuning-time
tags:
  - ia
lang: en
level: advanced
format: article
reading_time: 10
summary: Une équipe de Dharma-AI montre que l'estimation par les FLOPs prédit mal la durée réelle d'un fine-tuning sur une grappe de GPU partagée. Un modèle `CatBoost` entraîné sur 272 exécutions atteint un R² de 0,98 à partir de la seule configuration soumise.
takeaways:
  - L'estimation par les FLOPs n'explique qu'une petite partie de la durée réelle (R² de 0,31 au mieux)
  - Les évaluations périodiques (`num_eval_data`) et les checkpoints pèsent lourd dans le temps total
  - Un modèle `CatBoost` nourri de la seule configuration prédit la durée à 1,7 h près en moyenne
  - "Les résultats valent pour le SFT sur H200 : il faut réentraîner le modèle pour un autre matériel"
status: rejected
draft: true
featured: false
---
La méthode habituelle pour estimer la durée d'un fine-tuning repose sur un calcul de FLOPs : six fois le nombre de paramètres multiplié par le nombre de tokens, divisé par la puissance de crête des GPU et par le taux d'utilisation `MFU` (Model FLOPs Utilization), généralement compris entre 30 % et 60 %.

```text
T_theoretical = (6 · P · D) / (N_GPU · FLOPS_peak · MFU)
```

Les auteurs expliquent pourquoi cette estimation se trompe : un job ne se limite pas à sa boucle d'optimisation. La préparation, le téléchargement et le chargement du modèle, la tokenisation, les évaluations périodiques, l'envoi des checkpoints et l'écriture finale ajoutent du temps. Ces surcoûts dépendent de la configuration et se disputent les entrées-sorties sur une grappe partagée. Sur 272 exécutions de fine-tuning supervisé (SFT), menées sur des GPU NVIDIA H200 en production, soit environ 7 382 heures-GPU pour des durées allant de 1,6 à 172,4 h, la référence FLOPs n'obtient qu'un R² de 0,31 et une erreur absolue moyenne de 12,41 h avec un `MFU` de 30 %. Calculon, un modèle analytique de performance que les auteurs ont étendu à LoRA, à la GQA, à `bfloat16` et à SwiGLU, fait pire : R² de −0,51, avec des estimations qui ne couvrent en médiane que 11 % du temps observé.

L'équipe traite donc la durée comme un problème de régression supervisée. Le modèle n'utilise que les informations disponibles avant le lancement : hyperparamètres, taille des données, identifiants du modèle, nombre de GPU et pile logicielle (Transformers ou Unsloth). Des variables dérivées décrivent les surcoûts, comme `num_eval_data` (le nombre d'échantillons traités par l'ensemble des évaluations), `num_saves` et `num_params_saved` (les entrées-sorties des checkpoints). L'estimation FLOPs elle-même est fournie au modèle sous le nom `baseline_time`. En validation croisée stratifiée à cinq plis, `CatBoost` atteint un R² de 0,98 et une erreur absolue moyenne de 1,71 h (médiane de 0,51 h). `XGBoost` arrive juste derrière, tandis que Ridge et la régression linéaire plafonnent autour d'un R² de 0,7. La variable la plus importante est `num_eval_data`, suivie du nombre de paramètres entraînables.

Ce prédicteur doit servir à l'ordonnancement : choisir les jobs à exécuter ensemble, prévoir quand des GPU se libèrent et vérifier si une exécution tiendra son échéance. L'étude a ses limites : elle ne couvre que le SFT (ni RLHF, ni DPO), un seul type de GPU et seulement 13 exécutions de plus de 60 h, durées pour lesquelles l'erreur grandit. Selon les auteurs, c'est la méthode qui se transpose, pas le modèle entraîné. Un article plus complet doit paraître sur arXiv.
