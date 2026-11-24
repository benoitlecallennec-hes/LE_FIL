---
title: "180 milliards de paramètres dans 32 Go de RAM : Transformers charge les experts MoE depuis le disque"
date: 2026-11-18T00:00:00.000Z
source: Hugging Face Blog
link: https://huggingface.co/blog/moe-expert-streaming
tags:
  - ia
lang: en
level: advanced
format: article
summary: Tarek Ziadé (Hugging Face) fait tourner Qwen3.8-Flash-Next, un modèle MoE de 180 milliards de paramètres, sur un MacBook M5 doté de 32 Go de mémoire. Dans Transformers, seuls les experts choisis par le routeur restent dans un cache borné sur l'accélérateur ; les autres sont lus sur le disque à la demande.
takeaways:
  - Seuls les experts choisis par le routeur restent dans un cache sur l'accélérateur (`cache_bytes`) ; les autres sont lus sur le disque à la demande, avec un préchargement guidé par les routeurs des couches suivantes.
  - Un pool de slots partagé permet de réutiliser le noyau `grouped_mm` sans écrire de produit matriciel spécifique aux experts.
  - Un modèle de 180 milliards de paramètres décode à 13,9 tokens/s sur un MacBook M5 de 32 Go, à 6 % de Strata.
  - "La fonction est proposée dans la PR #50717 ; l'API (`experts_offload`, `device_map={\"experts.*\": \"disk\"}`) n'est pas encore définitive."
missing:
  - image principale (og:image)
  - temps de lecture
status: published
featured: false
edition: 2026-W48
---
Les experts représentent plus de 90 % des poids d'un modèle Mixture-of-Experts, mais chaque token n'en sollicite qu'une petite partie. La technique d'« expert streaming » garde les experts fréquemment utilisés dans un cache sur l'accélérateur et lit les autres sur le disque. Le cache LRU par couche du premier prototype a été remplacé par un pool de slots : un seul tenseur préalloué, partagé par toutes les couches, avec une table par couche qui associe chaque expert à un slot. Les identifiants d'experts produits par le routeur sont convertis en numéros de slots. Le pool ressemble ainsi à une banque d'experts ordinaire, et le noyau `grouped_mm` existant s'y applique sans modification. Un défaut de cache déclenche une lecture contiguë par expert, répartie sur 8 threads d'entrée-sortie. Les routeurs des deux couches suivantes sont exécutés en avance pour deviner quels experts précharger. Enfin, les experts peuvent être quantifiés hors ligne, ici sur 3 bits, pour réduire le volume lu.

Le modèle compte 48 couches de 512 experts, dont 10 sont actifs par token. En FP8, les experts pèsent 121 Go ; quantifiés sur 3 bits, ils occupent 49 Go sur le disque, dont 12 Gio sont gardés en cache sur l'accélérateur. Les couches denses font environ 5,4 Go en 8 bits, et la table d'embeddings n-grammes de 51 Go reste sur le disque, lue ligne par ligne au besoin. Une fois le modèle chargé, la mémoire occupée tourne autour de 18 Gio, avec un pic à 25,4 Gio pour un prompt de 6 000 tokens. Pour gagner du temps, l'auteur soumet tôt le travail GPU sur MPS, ce qui rapporte 10 %. Il a aussi écrit des noyaux Metal pour Gated DeltaNet et les hyper-connections, compilés à l'exécution avec `torch.mps.compile_shader`. Le préremplissage traite des blocs plus grands en lisant en parallèle les experts de la couche suivante. Sur les longs documents, l'attention se limite aux blocs retenus par l'indexeur, et le cache KV grandit sur place.

```python
model = AutoModelForCausalLM.from_pretrained(
    f"{path}/dense",
    dtype=torch.bfloat16,
    device_map={"": "mps", "experts.*": "disk"},
    quantization_config=MetalConfig(bits=8),
    experts_offload={
        "cache_bytes": 12 << 30,
        "quantized_experts": f"{path}/experts-q3",
    },
)
```

Sur le M5, Transformers atteint 13,9 tokens/s en décodage, contre 14,8 pour le moteur Strata. Le préremplissage d'un prompt de 4 000 tokens monte à 228 tokens/s, contre 220 pour Strata. Avant ces optimisations, Transformers plafonnait à 10 tokens/s en décodage et à 68 tokens/s en préremplissage. L'auteur a aussi comparé les moteurs sur Qwen3.6-35B-A3B avec un seul A10G de 24 Go. En décodage, Strata en Q8_0 fait 82 à 88 tokens/s, llama.cpp avec `--fit on` 63 à 64, et vLLM avec `--cpu-offload-params experts` au mieux 20,1. Transformers avec Accelerate tombe à 0,16 token/s. La fonction est proposée dans la PR #50717, et son API, dont l'option `experts_offload`, est encore en discussion avec les mainteneurs. Plusieurs limites demeurent : environ 10 ms par token partent en allers-retours avec l'hôte, et les défauts de cache ne sont pas masqués. La méthode ne s'applique qu'aux modèles MoE dont les experts suivent la disposition standard. La conversion du modèle prend environ 5 heures, et sur un Mac de 32 Go il faut fermer le navigateur pour éviter que macOS ne se mette à utiliser le swap.
