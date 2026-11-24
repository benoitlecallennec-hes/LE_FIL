---
title: "Inkling 2 Mini : le transformer en boucle de Thinking Machines Lab s'installe sur les machines locales"
date: 2026-11-20T00:00:00.000Z
source: Hugging Face Blog
link: https://huggingface.co/blog/inkling2mini
tags:
  - ia
lang: en
level: intermediate
format: article
image: https://huggingface.co/buckets/huggingface-projects/inkling-2-mini-release-assets/resolve/architecture.png
summary: Hugging Face et Thinking Machines Lab présentent Inkling 2 Mini. Ce modèle multimodal de type Mixture-of-Experts compte environ 26 milliards de paramètres, dont 3 milliards actifs par token. Il accepte un contexte d'un million de tokens et tourne en local avec `llama.cpp`, `transformers`, MLX ou SGLang.
takeaways:
  - MoE d'environ 26 milliards de paramètres, dont 3 milliards actifs, avec 12 couches centrales parcourues trois fois en boucle
  - Contexte d'un million de tokens, texte et images en entrée, décodage spéculatif DSpark
  - "En local, la quantification `IQ4_NL` tient dans 32 Go : `llama serve -hf ggml-org/Inkling-2-Mini-GGUF:IQ4_NL`"
  - Effort de raisonnement réglable par `reasoning_effort` dans `apply_chat_template`
missing:
  - scores chiffrés des benchmarks (dans des images)
  - mesures de débit du décodage spéculatif (graphique)
  - licence du modèle
  - fiche du modèle non consultée
  - balise og:image
status: published
featured: false
edition: 2026-W48
---
Inkling 2 Mini est une version bien plus petite qu'Inkling 2 Small, pensée pour tourner en local. Il s'agit d'un « transformer en boucle » de type Mixture-of-Experts, avec environ 26 milliards de paramètres physiques et 3 milliards actifs par token. Le modèle compte 24 couches de décodeur. Les couches 0 à 5 et 18 à 23 s'exécutent une seule fois, et les douze couches du milieu (6 à 17) sont parcourues trois fois, ce qui fait 48 passages logiques par token. À chaque passage, le routeur choisit 8 experts SwiGLU parmi 128. Dans chaque bloc de six couches, cinq couches utilisent une attention à fenêtre glissante (jusqu'à 1 024 tokens) et une couche une attention complète. Grâce à des « messages » entre passes, chaque tour de boucle peut lire les sorties du tour précédent par des portes apprises. Les poids des experts sont stockés en NVFP4 et MXFP8. Le modèle prend en entrée du texte et des images, produit du texte et gère un contexte d'un million de tokens. Il est livré avec un modèle brouillon DSpark pour le décodage spéculatif.

Selon le billet, le modèle est le meilleur de sa catégorie sur l'Artificial Analysis Intelligence Index v5. Face à Gemma 4 31B, Qwen3.8-27B ou Nemotron 3.5 Super VL, il arrive en tête sur 32 benchmarks sur 48. En code, il mène sur FrontierCode v2.0, fait jeu égal avec Qwen3.8-27B sur Terminal-Bench 4.0 et reste derrière lui sur AutomationBench-AA. Une variante de recherche, `TML-d24-varloop-research`, peut faire 1, 2 ou 3 passes de boucle, ce qui permet d'échanger un peu de qualité contre de la vitesse.

Pour un usage local, le billet recommande la quantification GGUF `IQ4_NL`, qui tient dans 32 Go de mémoire GPU ou unifiée. `Q8_0` est quasiment sans perte mais demande plus de mémoire. Avec `llama.cpp`, la commande `llama serve -hf ggml-org/Inkling-2-Mini-GGUF:IQ4_NL` lance un serveur compatible OpenAI. Côté `transformers`, on charge le modèle avec `AutoModelForMultimodalLM` et `AutoProcessor`. L'effort de raisonnement se règle par `reasoning_effort` dans `apply_chat_template`, de `none` à `max` ou par une valeur entre 0.0 et 0.99. Le support est aussi intégré à `mlx-lm`. En datacenter, SGLang prend le modèle en charge dès sa sortie, sur `SM103` :

```bash
python -m sglang.launch_server \
  --model-path $MODEL \
  --served-model-name inkling \
  --attention-backend fa4 --bf16-gemm-backend torch --dtype bfloat16 \
  --tp 1 --page-size 128 --grammar-backend xgrammar \
  --host 0.0.0.0 --port 30000
```

Pour activer le décodage spéculatif, on ajoute `--speculative-algorithm DSPARK --context-length 131072`, et `--enable-multimodal` pour les images. Le billet présente aussi plusieurs démonstrations : shaders, recréation WebGL de *La Nuit étoilée*, application construite à partir d'un croquis, et agent de navigation web.
