---
title: "DaisyRose-27B : RadixArk affine Qwen3.8-27B par apprentissage par renforcement sur de longues tâches d'agent"
date: 2026-11-18T19:54:56.000Z
source: Hugging Face Blog
link: https://huggingface.co/blog/RadixArk/daisyrose-27b-pushing-the-frontier-of-post-trainin
tags:
  - ia
lang: en
level: advanced
format: article
image: https://www.radixark.com/images/daisyrose-27b/hero.png
summary: Premier modèle de RadixArk, DaisyRose-27B part de Qwen3.8-27B et a été entraîné par apprentissage par renforcement avec Miles sur des tâches d'agent de longue durée. Son score pass@1 sur Terminal-Bench 4.1 passe de 5,6 % à 12,0 %, et l'entraînement coûte 57 % de temps GPU en moins.
takeaways:
  - Le score pass@1 sur Terminal-Bench 4.1 passe de 5,6 % à 12,0 %, avec des gains sur tous les autres benchmarks publiés.
  - Le MFU est de 37 % contre 16 % pour un entraîneur basé sur Megatron, ce qui réduit le temps GPU de 57 %.
  - "Miles tourne en mode asynchrone : 128 H200 pour l'inférence des agents et 16 pour l'entraînement, avec des sessions jusqu'à 1M de tokens."
  - Le modèle est publié sous Apache 2.0 en BF16, FP8 et NVFP4 et se sert avec SGLang ; le raisonnement arrive dans `reasoning_content`.
missing:
  - image og:image non confirmée (image principale relevée dans la page)
status: rejected
draft: true
featured: false
---
DaisyRose-27B est publié sous licence Apache 2.0, en points de contrôle BF16, FP8 et NVFP4. Il conserve l'architecture, le tokenizer, le modèle de chat, la compréhension d'images et la fenêtre de contexte de 256K tokens de Qwen3.8-27B. L'entraînement a duré 18 jours sur 144 GPU H200 et porte sur 1 200 tâches sélectionnées avec Eigent AI parmi des milliers de candidates, en apprentissage automatique, génie logiciel et sciences. Chaque tâche vise une erreur que commet le modèle d'origine et demande plusieurs étapes qui dépendent les unes des autres : jusqu'à 1,5 heure en bac à sable, plus de 100 appels d'outils et plus de 256K tokens de sortie par tentative. Les tâches qu'on pouvait réussir sans résoudre le vrai problème ont été corrigées ou retirées.

Miles tourne en mode entièrement asynchrone. 128 H200 exécutent l'inférence des agents et 16 autres l'entraîneur ; après chaque étape, les poids mis à jour sont envoyés aux GPU d'inférence. Les sessions vont jusqu'à 1M de tokens et sont découpées en plusieurs trajectoires grâce à un résumé fait par le harnais. Sur le backend interne de RadixArk, le MFU atteint 37 %, contre 16 % pour l'entraîneur open source basé sur Megatron. Le débit de bout en bout est d'environ 16 800 tokens/s, contre 8 300. Sur des modèles MoE, le gain va de 6,2× sur Qwen3.6-35B-A3B à plus de 10× sur GLM-5.3-Flash. Le service de bacs à sable, bâti sur AgentENV, a créé environ 944 000 bacs à sable en neuf semaines, avec 99 % de créations réussies. Il coûte environ 80 % de moins que les fournisseurs cloud courants.

Le modèle dépasse Qwen3.8-27B sur tous les benchmarks publiés : Terminal Bench 2.1 (73,0 → 75,5), DeepSWE v1.1 (42,2 → 46,2), SWE Bench Pro v2 (91,3 → 94,7), IFBench (79,5 → 82,4) et GPQA Diamond (89,2 → 90,7). Pour le servir, RadixArk recommande SGLang avec le décodage spéculatif DSpark, puis un client compatible OpenAI. Par défaut, le modèle raisonne avant de répondre : le raisonnement arrive dans `reasoning_content` et la réponse dans `content`.

```python
from openai import OpenAI

client = OpenAI(base_url="http://localhost:30000/v1", api_key="none")

response = client.chat.completions.create(
    model="DaisyRose-27B",
    messages=[{"role": "user", "content": "How many positive divisors does 360 have?"}],
    temperature=1.0,
    top_p=0.95,
    extra_body={"top_k": 20, "min_p": 0.0},
)
```
