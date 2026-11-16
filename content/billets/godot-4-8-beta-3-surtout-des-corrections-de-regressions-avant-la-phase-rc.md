---
title: "Godot 4.8 beta 3 : surtout des corrections de régressions avant la phase RC"
date: 2026-11-09T12:00:00.000Z
source: godotengine.org
link: https://godotengine.org/article/dev-snapshot-godot-4-8-beta-3/
tags:
  - 3d
feed_tag: Pre-release
lang: en
level: intermediate
format: article
reading_time: 3
image: https://godotengine.org/storage/blog/covers/dev-snapshot-godot-4-8-beta-3.jpg
summary: "Troisième bêta de Godot 4.8 : 74 correctifs de 51 contributeurs, surtout des régressions. L'équipe se rapproche de la phase release candidate et demande aux utilisateurs de signaler les bugs dès maintenant."
takeaways:
  - "Godot 4.8 beta 3 : 74 correctifs par 51 contributeurs, surtout des régressions"
  - "La phase release candidate approche : c'est le moment de signaler les bugs"
  - Correction d'une condition de course dans `CommandQueueMT` (physique multithread)
  - Nouvelle classe abstraite `LineBase3D` et build IDs pour les versions Linux et Windows avec symboles de débogage
status: published
featured: false
edition: 2026-W47
---
Malgré une partie de l'équipe mobilisée par la GodotFest, le développement de Godot 4.8 a continué sur les derniers bugs qui bloquent la sortie. Les développeurs pensent passer bientôt à la phase release candidate et invitent donc à tester cette beta 3 pour signaler les bugs et régressions. Elle est disponible pour Linux, macOS et Windows (versions Standard et .NET), ainsi qu'en éditeur Web, XR et Android.

Le billet ne présente que les changements depuis la beta 2, qui sont surtout des corrections de régressions. En 3D, une classe abstraite `LineBase3D` fait son apparition. En physique, une condition de course a été corrigée : elle survenait quand plusieurs threads vidaient la `CommandQueueMT` en même temps. Côté shaders, le nœud de sortie du `VisualShader` reçoit une entrée « light vertex ». Le système de build active les build IDs pour les versions Linux et Windows avec symboles de débogage.

D'autres corrections touchent l'animation, où les clés ne sont plus insérées automatiquement quand l'`Animation` n'est pas active. Dans l'éditeur, l'option « Save on Focus Loss » ne recrée plus les scènes ouvertes qui ont été supprimées. Pour l'interface et les entrées, `TabContainer` n'affiche plus le mauvais onglet quand il n'est pas dans l'arbre, et la souris ne prend plus le pas sur la manette dans les menus. Le message d'erreur de `Sprite2D` est plus clair quand la texture est nulle, et le pilote audio Sample pour le Web a reçu plusieurs correctifs.

Au total, 51 contributeurs ont soumis 74 correctifs pour cette version, construite à partir du commit `c51ff06e8`. Les nouveautés générales de la version 4.8 sont présentées dans l'annonce de la beta 1.
