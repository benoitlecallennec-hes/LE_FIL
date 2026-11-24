---
title: "Rumble-2-Flash : un modèle de reconnaissance vocale ouvert de 0,5 milliard de paramètres pour plus de 40 langues"
date: 2026-11-18T00:00:00.000Z
source: Hugging Face Blog
link: https://huggingface.co/blog/rumble-2-flash
tags:
  - ia
lang: en
level: intermediate
format: article
summary: Reson8 publie sous licence Apache 2.0 Rumble-2-Flash, un modèle de transcription de la parole de 0,5 milliard de paramètres. Il se charge directement dans 🤗 Transformers et vLLM, sans `trust_remote_code`, et rivalise avec des modèles 3 à 60 fois plus gros.
takeaways:
  - Un modèle ASR ouvert de 0,5 milliard de paramètres, sous Apache 2.0, pour plus de 40 langues, avec détection automatique de la langue.
  - L'architecture Qwen3-ASR permet de l'utiliser directement dans 🤗 Transformers et vLLM, sans `trust_remote_code`.
  - Il est distillé « on-policy » depuis Rumble-2, avec GOLD pour aligner des tokeniseurs différents.
  - Il obtient 6,96 % d'erreur moyenne en multilingue sur l'Open ASR Leaderboard et dépasse 7 000 RTFx quand les lots restent pleins.
missing:
  - image principale de la page
  - temps de lecture
status: rejected
draft: true
featured: false
---
Rumble-2-Flash est un modèle de reconnaissance automatique de la parole (ASR) à poids ouverts, publié par Reson8 sous licence Apache 2.0. Il compte 0,5 milliard de paramètres et couvre plus de 40 langues. Il détecte la langue tout seul et laisse régler la sortie : nombres en chiffres ou en lettres, mots de remplissage, répétitions verbatim ou langue imposée. Il reprend l'architecture Qwen3-ASR, ce qui lui assure un support immédiat dans 🤗 Transformers et vLLM, sans runtime dérivé ni `trust_remote_code`.

Le modèle est distillé depuis Rumble-2, le modèle de production plus gros de l'équipe, par distillation « on-policy ». L'élève transcrit lui-même l'audio, puis le professeur note chaque token selon la probabilité qu'il lui donne. Les deux modèles ne découpent pas le texte de la même façon. La méthode GOLD fusionne donc les tokens là où les découpages diffèrent. Les tokens communs sont comparés directement, les autres par leurs probabilités triées.

Sur la piste multilingue de l'Open ASR Leaderboard, le modèle obtient un taux d'erreur moyen de 6,96 % sur neuf langues : chinois, allemand, français, italien, espagnol, portugais, hindi, néerlandais et anglais. Il devance omniASR-LLM 7B de Meta, quinze fois plus gros, ainsi que Whisper large-v3. Sur la piste anglaise, son WER moyen est de 6,6 %. Quand les lots restent pleins, il dépasse 7 000 RTFx, soit environ une heure d'audio transcrite en une demi-seconde. Le benchmark AppTek réunit 302 heures d'appels de centres d'assistance, avec 14 accents anglais. Rumble-2-Flash y atteint 7,86 % de WER, contre 7,78 % pour Nemotron 3.5 Transcribe de NVIDIA (30 milliards de paramètres). Seul Qwen3-ASR 1.7B fait mieux, avec 7,4 %.

Pour l'utiliser, il suffit d'installer `torch`, `transformers` et `accelerate`. On passe ensuite par `apply_transcription_request`, dont le paramètre `audio` accepte un chemin local, une URL, un tableau NumPy ou une liste de ces éléments. Si on omet `language`, la langue est détectée automatiquement, et il faut alors décoder avec `return_format="parsed"`.

```python
from transformers import AutoProcessor, AutoModelForMultimodalLM

model_id = "Reson8/Rumble-2-Flash"
processor = AutoProcessor.from_pretrained(model_id)
model = AutoModelForMultimodalLM.from_pretrained(model_id, device_map="auto")

inputs = processor.apply_transcription_request(
    audio="https://hf.co/datasets/hf-internal-testing/dummy-audio-samples/resolve/main/bcn_weather.mp3",
    language="English",  # omit to detect automatically
).to(model.device, dtype=model.dtype)

output = model.generate(**inputs, max_new_tokens=512)
generated = output[:, inputs["input_ids"].shape[-1]:]
print(processor.decode(generated, return_format="transcription_only")[0])  # return_format="parsed" if detecting language
```

Le modèle est aussi disponible via l'API de Reson8.
