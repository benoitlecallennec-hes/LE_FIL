---
title: "Agent skills : écrire une compétence une fois et la réutiliser avec n'importe quel agent"
date: 2026-11-12T22:54:52.000Z
source: The GitHub Blog
link: https://github.blog/ai-and-ml/github-copilot/build-once-run-on-any-agent-a-practical-guide-to-agent-skills/
tags:
  - ia
feed_tag: AI & ML
lang: en
level: intermediate
format: article
reading_time: 6
image: https://github.blog/wp-content/uploads/2026/11/5b5fed0ce69f241f469053d4f178383688adb2fb7ef44dff82c1a2dd6a626fa1-1920x1080-1.png
summary: "Sur le blog de GitHub, Andrea Griffiths explique comment écrire des agent skills, des dossiers versionnés construits autour d'un fichier `SKILL.md`. Avec deux exemples, Proof Agent et Data Forge, l'article montre comment les rendre réutilisables d'un agent à l'autre : Claude Code, Cursor ou GitHub Copilot."
takeaways:
  - Une skill tient dans un dossier versionné autour de `SKILL.md`, avec une seule tâche et des conditions d'usage explicites.
  - Les détails propres au projet vont dans un manifeste partagé, pas dans les instructions de la skill.
  - Les instructions guident le travail ; la vérification revient aux revues humaines, aux tests, à la CI et aux règles du dépôt.
  - Un script de packaging génère, à partir des mêmes fichiers `SKILL.md`, les fichiers propres à Claude Code, Cursor et Copilot.
status: published
featured: false
edition: 2026-W48
---
Une agent skill est un dossier versionné construit autour d'un fichier `SKILL.md`. Ce fichier décrit quand et comment accomplir une tâche répétitive, pour ne plus avoir à la réexpliquer à l'agent ou à un collègue. La skill peut servir à plusieurs agents, mais c'est l'environnement qui fournit les outils, les identifiants et les permissions : une skill peut décrire comment interroger une base de données sans donner accès à cette base. L'article reprend une session de GitHub Universe 2026, présentée avec Shishir Tewari, de Procore. Premier conseil : donner à chaque skill une seule tâche, avec une entrée et une sortie claires. Proof Agent en est l'exemple. Cette skill confie la relecture d'un changement produit par une IA à un vérificateur indépendant, selon le principe que « s'auto-vérifier n'est pas vérifier ». Sa section `When to Verify` précise quand l'invoquer.

Data Forge montre comment plusieurs skills se coordonnent pour bâtir un pipeline de données. `forge-init` découvre les tables sources et les conventions de nommage, puis les écrit dans un manifeste. `forge-builder` mène le travail de l'issue jusqu'à la pull request : il s'appuie sur `forge-dbx` pour interroger la plateforme et sur `forge-lint` pour appliquer les normes de l'équipe. Une personne valide les décisions en chemin, et la pull request contient le code du pipeline, les changements de schéma et les tests. La configuration du projet reste hors des skills, dans le manifeste :

```yaml
project:
  name: "support-desk"
  store_name: "support_metric_store"

platform:
  engine: "duckdb"
  pipeline_framework: "spark_python"
  duckdb_dir: "warehouse"
```

L'auteure propose un test de portabilité : une autre équipe pourrait-elle utiliser ces skills avec son propre manifeste, sans les réécrire ? `forge-dbx` choisit un adaptateur Databricks ou DuckDB selon le manifeste, mais n'installe rien et ne donne aucun accès : « le manifeste choisit le moteur ; l'hôte fournit l'accès ». Le workflow de Data Forge compte sept phases, chacune avec un point de contrôle humain, et demande une approbation avant toute modification de schéma. Dans Proof Agent, une GitHub Action ne transmet au vérificateur que la demande d'origine et le diff. Elle en fait une revue statique, sans exécuter le code, et rend `PASS`, `FAIL` ou `PARTIAL` en citant des preuves. Par défaut, un `FAIL` fait échouer le check ; la démo utilisait `block-on-fail: false` pour que les résultats restent indicatifs.

Les fichiers `skills/*/SKILL.md` servent de source unique. Un script génère les fichiers propres à chaque client : `.claude-plugin/plugin.json` pour Claude Code, `.cursor-plugin/plugin.json` pour Cursor et `.github/agents/*.agent.md` pour GitHub Copilot. Le script dispose d'un mode de vérification pour la CI, `node scripts/package.mjs --check`. Une fois la skill installée, il faut vérifier que le client la reconnaît pour la bonne tâche, charge ses références, peut utiliser les outils nécessaires et rend les preuves demandées. Les skills conviennent au travail borné et répétitif : checklists, releases, migrations. Dès qu'il faut une interface stable, un état partagé, une piste d'audit ou des permissions appliquées hors du modèle, mieux vaut écrire une application.
