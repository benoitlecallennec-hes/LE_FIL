---
title: "Hugging Face présente les harness adapters : des LoRA entraînés par RL pour adapter un modèle à son agent de code"
date: 2026-11-20T00:00:00.000Z
source: Hugging Face Blog
link: https://huggingface.co/blog/harness-adapters-at-scale
tags:
  - ia
lang: en
level: advanced
format: article
summary: Hugging Face entraîne par apprentissage par renforcement un adaptateur LoRA par harnais d'agent (Pi, OpenCode, Hermes Agent) pour `Qwen3.8-27B`. Sur Terminal-Bench 2.1 et SWE-bench Pro, le modèle résout plus de tâches et appelle nettement moins d'outils.
takeaways:
  - Un harness adapter est un LoRA entraîné par RL (GRPO) dans un harnais d'agent précis, qui tourne tel quel, sans modifier les poids du modèle de base.
  - Seules les tâches que le modèle réussit parfois servent à l'entraînement, car GRPO a besoin de variance pour apprendre.
  - Sur Terminal-Bench 2.1 et SWE-bench Pro, les adaptateurs gagnent jusqu'à 25 points avec 53 à 72 % d'appels d'outils en moins sur les tâches résolues.
  - Les trois adaptateurs, presque orthogonaux, fusionnent avec `combination_type="cat"` de PEFT en un super-adaptateur valable pour plusieurs harnais.
missing:
  - résultats chiffrés de l'ablation sur les filtres
  - image principale de la page
status: published
featured: false
edition: 2026-W48
---
Un modèle généraliste n'est pas optimisé pour chaque harnais d'agent, c'est-à-dire l'infrastructure qui le fait tourner comme agent, ni pour chaque type de tâche. L'équipe de Hugging Face propose donc des « harness adapters » : des adaptateurs LoRA entraînés par apprentissage par renforcement (GRPO) à l'intérieur d'un harnais précis, sans toucher aux poids du modèle de base. Le harnais est utilisé tel quel. Un proxy placé entre lui et vLLM enregistre les tokens exacts produits par le modèle, et l'entraînement apprend de ces tokens. D'autres frameworks réimplémentent la boucle d'agent ; ici, ce n'est pas le cas. Exemple mis en avant : sous Pi, `Qwen3.8-27B` passe de 39 % à 55 % sur Terminal-Bench 2.1, et sa consommation moyenne tombe de 2,9K à 1,5K tokens par tâche. Selon les auteurs, l'essentiel du gain vient de ce que le modèle raisonne moins, appelle moins d'outils et réussit du premier coup.

Les tâches d'entraînement sont au format Harbor, celui de Terminal-Bench 2.1. Elles viennent de trois sources : SWE-smith (bugs injectés), SWE-Gym (vraies issues GitHub) et Tasksmith (tâches générées à partir de pull requests fusionnées). Chaque tâche est validée par `openenv validate`, puis filtrée selon sa difficulté. Le modèle de base tente chaque tâche plusieurs fois, et seules celles qu'il réussit parfois sont gardées, car GRPO a besoin de variance au sein d'un groupe pour apprendre. Le jeu final, `harness-hub/agentic-coding-envs`, compte 871 tâches issues de 35 dépôts, dont 171 « qualifiées ».

```bash
openenv validate path/to/task --sandbox hf-sandbox
```

L'entraînement utilise le `AsyncGRPOTrainer` de TRL, avec un LoRA `all-linear` sur deux GPU B200 (un pour vLLM, un pour l'entraînement). La récompense combine la justesse vérifiée et un bonus d'efficacité dans l'usage des outils. Le mode réflexion est désactivé pendant les rollouts et le taux d'apprentissage final vaut `2e-5`. Le plugin `safeagents` bloque les appels d'outils problématiques : 647 blocages sur 1,3 million d'appels. Les trois adaptateurs sont presque orthogonaux. Ils ont été fusionnés en un « super-adaptateur » de rang 96 grâce au `combination_type="cat"` de PEFT, avec un poids de ½.

Avec son adaptateur, Pi passe de 39,2 % à 64,7 % sur les 51 tâches les plus difficiles de SWE-bench Pro. Sur Terminal-Bench 2.1, le gain est de 16,3 points pour Pi, 14,1 pour Hermes et 6,0 pour OpenCode. Sur les tâches que les deux modèles résolvent, les adaptateurs font 53 à 72 % d'appels d'outils en moins. Le super-adaptateur fait passer la moyenne de 39,5 % à 49,0 % sur six couples harnais-benchmark. Il bat aussi le modèle de base en mode réflexion (63,5 % contre 55,5 %). Un adaptateur entraîné pour Pi améliore aussi les résultats sous OpenCode et Hermes. En revanche, sur AutomationBench, hors de la distribution d'entraînement, les écarts restent dans le bruit. Question encore ouverte : en mode sans réflexion, les adaptateurs écrivent souvent un court texte avant les appels d'outils, une forme de « fuite de raisonnement ».
