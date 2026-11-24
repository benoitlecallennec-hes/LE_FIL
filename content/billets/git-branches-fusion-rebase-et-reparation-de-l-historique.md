---
title: "Git : branches, fusion, rebase et réparation de l'historique"
date: 2026-11-24T16:25:09.843Z
source: Cours Génie logiciel
link: https://he-arc.github.io/2243.2-Genie_Logiciel/docs/cours/10_git_part2/
tags:
  - git
lang: fr
level: beginner
format: article
reading_time: 15
summary: Le chapitre 10 du cours de génie logiciel de la HE-Arc présente le travail avec les branches Git, l'intégration par `merge` ou `rebase` et les commandes qui permettent d'annuler ou de réparer, de `git restore` à `git reflog`.
takeaways:
  - "`git switch` et `git restore` remplacent les usages correspondants de `git checkout`, qui reste présenté comme forme classique."
  - "`git rebase` suivi de `git merge --ff-only` produit un historique linéaire."
  - Seuls `git restore` et `git revert` ne réécrivent pas l'historique ; `reset`, `commit --amend` et `rebase --interactive` le réécrivent.
  - "Règle d'or : ne jamais réécrire un historique déjà poussé, utiliser `git revert` à la place."
missing:
  - image principale de la page
status: published
featured: false
edition: 2026-W48
notions:
  - Git
  - Merge
  - Rebase
---
Le chapitre commence par une vue d'ensemble des quatre zones de Git : Working Tree, Index, `HEAD` et Remote. Il les relie aux commandes `restore`, `add`, `commit`, `push` et aux trois variantes de `reset`. Il montre ensuite comment créer, changer, lister et supprimer des branches, en local comme sur le serveur. Les commandes sont données sous leur forme moderne et sous leur forme classique, par exemple `git switch -c <nom>` et `git checkout -b <nom>`. Il explique aussi comment désigner un commit : par son hash, par une branche, ou par rapport à `HEAD` (`HEAD~1`, `HEAD~2`). Les plages de commits s'écrivent `<from>..<to>`, et `origin/main..HEAD` liste les commits qui restent à pousser.

La fusion suit une démarche en sept étapes, de la création de la branche jusqu'au `push`. Un même fichier modifié à la fois sur `main` et sur `DEV_Branch` sert d'exemple de conflit. Le cours montre la résolution dans VS Code (Accept Current, Incoming ou Both Changes) puis à la main, et la conclut par `git add <file>` et `git commit`. Vient ensuite `git rebase`, qui rejoue les commits d'une branche sur une autre. Il peut aussi provoquer des conflits, mais il produit un historique linéaire. Le cours détaille le déroulé complet et cite `git rebase --interactive`, qui permet de modifier plusieurs commits à la fois.

```bash
git switch main
git pull --rebase
git switch <Dev>_<Id>_<Summary>
git rebase main
git switch main
git merge --ff-only <Dev>_<Id>_<Summary>
git push origin main
git branch --delete <Dev>_<Id>_<Summary>
```

La dernière partie porte sur l'annulation et la réparation. `git restore --worktree <file>` remet le fichier à sa version de l'index, et `git restore --staged <file>` le retire de l'index sans perdre les modifications. `git reset` existe en trois variantes : avec `--soft`, les modifications restent dans l'index ; avec `--mixed`, elles restent dans le Working Tree ; avec `--hard`, elles sont perdues. `git commit --amend` modifie le dernier commit. `git revert <commit>` crée un nouveau commit qui annule le commit visé. Enfin, `git reflog` garde la trace de chaque déplacement de `HEAD`, ce qui permet de retrouver un commit « perdu » après un `reset --hard` et de le rattacher à une nouvelle branche.
