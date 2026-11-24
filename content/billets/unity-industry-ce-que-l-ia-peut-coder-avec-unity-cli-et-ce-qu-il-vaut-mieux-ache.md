---
title: "Unity Industry : ce que l'IA peut coder avec Unity CLI, et ce qu'il vaut mieux acheter"
date: 2026-11-23T00:00:00.000Z
source: Unity Blog
link: https://unity.com/blog/ai-vibe-coding-vs-buying-managed-infrastructure
tags:
  - 3d
lang: en
level: beginner
format: article
reading_time: 15
image: https://mip.web.unity.com/img/4pUvuYg0gyB2bkIhoO5QHLkxTu_NMTSO2B5d9UajfaE/w:1568/h:941/r:52,0,1568,941/q:75/f:avif/aHR0cHM6Ly9jZG4uc2FuaXR5LmlvL2ltYWdlcy9mdXZiampscC9wcm9kdWN0aW9uL2U0N2QxYjdlODJkZmE5NzJjNjAzMWI5N2Q3ZjcyMWZjMDQzMWVkODYtMTY3MXg5NDEucG5n.avif
summary: Unity propose de séparer la « surface » (interfaces, scripts C#, outils d'éditeur, prototypes), que l'on peut confier au vibe coding avec la nouvelle Unity CLI, de la « fondation » (import CAO, gestion d'assets, sécurité, versionnage), qu'il vaut mieux acheter sous forme d'infrastructure gérée.
takeaways:
  - Unity CLI ouvre l'éditeur aux agents d'IA, et sa commande `Eval` exécute du C# sans rechargement de domaine
  - Interfaces (uGUI, UI Toolkit), outils d'éditeur, scripts ponctuels et prototypes se prêtent bien au vibe coding
  - Import CAO propriétaire, gestion des secrets, versionnage et conformité restent du ressort d'une infrastructure gérée
  - "Le vrai coût du code généré, c'est sa maintenance : tests, contrôle d'accès, « dette de compréhension »"
missing:
  - balise og:image non vérifiée (image d'en-tête reprise du corps de la page)
  - vidéos liées non consultées
status: selected
draft: true
featured: false
edition: 2026-W48
---
Les LLM écrivent désormais du code assez stable pour prototyper et livrer. Unity se demande donc ce qu'une équipe devrait construire elle-même avec l'IA et ce qu'elle devrait acheter. L'éditeur vient de lancer une interface en ligne de commande, Unity CLI, qui donne à un LLM un accès simplifié aux commandes clés et à l'exécution. Sa commande `Eval` exécute du C# directement dans l'éditeur Unity, sans rechargement de domaine. La CLI fonctionne avec les agents d'IA courants et avec les systèmes de CI et de build. L'article rappelle que Unity n'a pas eu besoin de refondre son moteur pour l'IA : ses API ouvertes, sa couche de script, son écosystème de packages et C# servaient déjà de points d'extension.

Le vibe coding donne ses meilleurs résultats sur la « surface », grâce à l'abondance de matériel public sur Unity. L'article cite trois exemples. Le premier est un tableau de bord automobile en uGUI, produit à partir d'un moodboard en quelques heures au lieu de plusieurs jours ou semaines. Le deuxième ajoute des commandes de robot et de caméra à un jumeau numérique d'entrepôt : pavés tactiles MOVE (WASD) et ORBIT, et bascule AUTO / MANUAL. Le troisième est un pipeline qui envoie une image 2D dans Unity Asset Manager, la transmet à un générateur 3D tiers, puis récupère l'asset produit.

La « fondation » pose d'autres problèmes. Les formats CAO natifs (CATIA, NX, Creo, SolidWorks) sont moins bien documentés que glTF, OBJ ou JSON, et un modèle ne sait donc pas les interpréter de façon fiable : les erreurs se logent dans la tessellation, la hiérarchie, les métadonnées et le positionnement des assemblages. Sur la sécurité, l'article cite plusieurs risques des pipelines faits maison : secrets qui fuient dans les logs, contrôle d'accès ajouté trop tard, permissions multi-utilisateurs improvisées. Il insiste aussi sur la « dette de compréhension » décrite par Addy Osmani : personne ne maîtrise plus vraiment la logique générée. Enfin, les coûts d'IA facturés à l'usage peuvent grimper et sont difficiles à plafonner.

Un tableau tranche cas par cas. On peut confier à l'IA les scripts ponctuels, les outils d'éditeur sur mesure, les visualiseurs, les interfaces et les preuves de concept. Le « peut-être » concerne la colle entre deux systèmes que l'on maîtrise. Le « non » s'applique à la lecture des formats CAO propriétaires, aux secrets et jetons, à la conversion CAO de production, au versionnage, à la gestion d'assets et à tout ce qui touche aux données clients ou à la conformité. Selon l'article, la difficulté n'est pas d'écrire le code mais d'en assurer la propriété dans la durée. Il recommande de combiner une fondation gérée et du vibe coding en surface.
