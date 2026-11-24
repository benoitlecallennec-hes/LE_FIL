---
title: Hugging Face explique comment construire et maintenir une évaluation d'agents
date: 2026-11-23T00:00:00.000Z
source: Hugging Face Blog
link: https://huggingface.co/blog/agentic-evals
tags:
  - ia
lang: en
level: intermediate
format: article
reading_time: 35
image: https://huggingface.co/blog/assets/agentic-evals/thumbnail.png
summary: Hugging Face publie un guide pour concevoir une évaluation d'agents fiable. Un exemple complet sur l'ajustement de spectres Raman, exécuté avec `harbor`, montre comment éviter les raccourcis, lire les traces, rendre compte des scores et décider de ce qu'on rend public.
takeaways:
  - "Avant de lire les échecs d'un modèle, tester l'évaluation elle-même : un oracle, un agent `nop`, des raccourcis paresseux et d'autres méthodes valides."
  - "Noter le résultat enregistré plutôt que la déclaration de fin de tâche : `gpt-oss-120b` appelait souvent `mark_task_complete` sans produire de fichier."
  - Rendre compte de pass@k ou pass^k avec une incertitude groupée, le coût et les tokens, et fixer le fournisseur pour pouvoir reproduire les résultats.
  - Ouvrir l'évaluation composant par composant sur le Hub et tenir un historique des corrections par version.
missing:
  - figures interactives (seules les légendes ont été lues)
  - temps de lecture non indiqué (estimé)
status: published
featured: false
edition: 2026-W48
---
Le guide part d'un constat : les évaluations orientent les systèmes que l'on construit et ceux auxquels on fait confiance. Il distingue trois époques : les benchmarks questions-réponses (MMLU, GPQA), les benchmarks d'agents (SWE-bench, WebArena), puis les évaluations d'équipes ou d'essaims d'agents, qu'il laisse de côté. Une évaluation d'agents se décompose en plusieurs éléments. La tâche donne la consigne. Le harnais d'agent entoure le modèle et ses outils dans un environnement figé. Un rollout est une tentative, et le harnais d'évaluation en lance plusieurs par tâche. Le vérificateur note l'état final, et chaque rollout laisse une trace. Le choix du harnais pèse lourd : sur Terminal-Bench, le changer en gardant le même modèle fait varier la précision de 29,9 points.

L'exemple suivi est un agent de laboratoire de physique. Il doit ajuster les pics d'un spectre Raman de graphène et calculer un rapport D/G qui mesure les défauts. Les auteurs partent de la tâche `raman-fitting` de Terminal-Bench, au format Harbor, et la transforment en `burtenshaw/raman-lab` : huit spectres synthétiques aux paramètres connus, trois pics à ajuster au lieu de deux. Avant de tester des modèles, ils vérifient l'évaluation elle-même avec une solution de référence (oracle) et un agent qui ne fait rien (`nop`).

```bash
harbor run -p datasets/my-evals -a oracle -e hf-sandbox
harbor run -p datasets/my-evals -a nop -e hf-sandbox
```

Un raccourci lisait la hauteur des pics directement dans les données et devinait leur largeur. Il donnait quand même le bon rapport sur les huit tâches. Les auteurs ont donc ajouté des contrôles sur chaque pic ajusté. Sur la tâche adaptée, `gpt-oss-120b` réussit 11 rollouts sur 24. Dans 19 d'entre eux, le modèle appelle `mark_task_complete` dès sa première réponse, et plusieurs échecs viennent de NumPy ou SciPy absents. Le score se lit en pass@3 (7/8) et pass^3 (1/8), avec une erreur standard groupée d'environ 0,11. En comparant les modèles avec un fournisseur d'inférence fixé, trois d'entre eux réussissent les 24 rollouts : ces tâches ne permettent donc pas de les départager.

Le guide insiste aussi sur les erreurs des benchmarks. Terminal-Bench 2.1 a corrigé 28 de ses 89 tâches après des signalements, et un audit de SWE-bench Verified a trouvé des problèmes sérieux dans 59,4 % des 138 problèmes examinés. Il recommande de consigner chaque erreur et ses versions concernées. Plutôt que de tout publier ou de tout garder privé, il conseille d'ouvrir chaque composant à part : tâches, environnements, vérificateurs, traces et résultats, publiés sur le Hub (`eval.yaml`, `.eval_results/`). Les éléments sensibles, comme les items de test actifs, restent protégés.
