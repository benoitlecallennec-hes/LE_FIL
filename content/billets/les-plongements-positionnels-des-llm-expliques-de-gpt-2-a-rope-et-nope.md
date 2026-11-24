---
title: Les plongements positionnels des LLM expliqués, de GPT-2 à RoPE et NoPE
date: 2026-11-15T12:10:37.000Z
source: Sebastian Raschka
link: https://magazine.sebastianraschka.com/p/understanding-positional-embeddings
tags:
  - ia
lang: en
level: advanced
format: article
reading_time: 35
image: https://substack-post-media.s3.amazonaws.com/public/images/2a036907-4c1e-473b-8d82-20f2c777df5f_2912x1632.png
summary: "Sebastian Raschka explique comment les LLM codent la position des tokens. Il part des plongements absolus appris de GPT-2, passe par les sinusoïdes du Transformer d'origine, détaille RoPE avec une implémentation PyTorch et finit sur les choix des modèles ouverts de 2026 : RoPE partiel, NoPE et biais relatif appris."
takeaways:
  - "La table de positions apprises de GPT-2 a une taille fixe et grossit avec le contexte : plus de 22 % des poids de GPT-2 Medium pour 100k positions."
  - RoPE tourne seulement `q` et `k`, d'un angle proportionnel à la position, si bien que le score d'attention ne dépend que de l'écart relatif.
  - "En 2026, de plus en plus de modèles ouverts adoptent le RoPE partiel (MiniMax-M3 : 64 dimensions sur 128), NoPE ou un biais relatif appris (Inkling)."
missing:
  - fin de la section 6 et annexes lues seulement via un résumé (YaRN non lu)
  - liste complète des modèles de 2026 (figure de la galerie)
  - image principale de la page
status: published
featured: false
edition: 2026-W48
---
Dans GPT-2, une table de vecteurs de position appris est ajoutée aux plongements des tokens avant le premier bloc transformer. Cette table est limitée à 1 024 positions, et sa taille doit être fixée avant l'entraînement. Pour GPT-2 Medium, elle pèse 0,30 % des poids à 1 024 positions, mais plus de 22 % pour un contexte de 100k tokens. Le Transformer d'origine fait la même addition avec des valeurs fixes en sinus et cosinus. Un décalage constant entre deux positions y correspond à une rotation constante de chaque paire de features. Raschka rappelle aussi pourquoi une position explicite reste utile : le masque causal donne un ordre implicite, mais il ne distingue pas un écart de 1 d'un écart de 10.

RoPE (Su et al., RoFormer, 2021) ne touche pas aux plongements d'entrée. Il fait tourner les vecteurs de requête et de clé d'un angle proportionnel à la position du token, dans chaque couche d'attention. Les valeurs ne sont pas tournées. Le produit scalaire ne dépend alors que de l'écart relatif entre les deux tokens. L'auteur le montre sur un exemple chiffré avec θ = 20° par position : décaler les deux positions de 3 laisse le score inchangé. Les fréquences suivent θ_i = b^(−2i/d), avec une base b = 10 000. L'implémentation part du code Qwen3 « from scratch » : `rope_params` construit les tables de cosinus et de sinus, et `rope_split_half` applique la rotation. Les features sont appariées par moitiés (1 avec 5, 2 avec 6…). La convention entrelacée du papier d'origine et de LLaMA v1 est équivalente, à un réordonnancement près.

```python
def rope_split_half(x, cos, sin):
    first, second = x.float().chunk(2, dim=-1)
    return torch.cat((
        first * cos - second * sin,
        first * sin + second * cos,
    ), dim=-1).to(x.dtype)
```

La dernière partie passe en revue les architectures récentes. MiniMax-M3 applique un RoPE partiel : seules 64 des 128 dimensions de chaque tête sont tournées, après une QK-Norm. D'après l'auteur, cela économise surtout de la mémoire dans les tables cos/sin, et il juge le RoPE partiel « à peine utile ». NoPE renonce à tout encodage explicite et laisse le masque causal porter la position. SmolLM3 n'utilise pas RoPE dans une couche sur quatre. Inkling emploie un biais de position relative appris, une famille qui compte aussi T5 et ALiBi. Dans la galerie de l'auteur, RoPE dans toutes les couches d'attention softmax reste le choix le plus courant, mais sa part a baissé en 2026. Le RoPE partiel est devenu majoritaire parmi les modèles RoPE de 2026.
