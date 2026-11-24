---
title: "Godot 4.8 beta 4 : la dernière ligne droite avant la version finale"
date: 2026-11-18T12:00:00.000Z
source: godotengine.org
link: https://godotengine.org/article/dev-snapshot-godot-4-8-beta-4/
tags:
  - 3d
feed_tag: Pre-release
lang: en
level: beginner
format: article
reading_time: 4
image: https://godotengine.org/storage/blog/covers/dev-snapshot-godot-4-8-beta-4.jpg
summary: Un peu plus d'une semaine après la beta 3, Godot 4.8 beta 4 corrige surtout des régressions. Ce pourrait être la dernière beta du cycle, donc la dernière occasion de signaler de nouveaux bugs.
takeaways:
  - Godot 4.8 beta 4 corrige surtout des régressions, et ce pourrait être la dernière beta avant la version finale.
  - "Parmi les correctifs : `VoxelGI` hors de bornes non uniformes, l'échelle des traînées 3D, les soulignements de diagnostic en GDScript et l'extension `.mesh` à l'import."
  - L'édition .NET de Godot 4.8 demande désormais .NET 10.0 ou plus, au lieu de .NET 8.
  - 58 contributeurs, 123 correctifs, version construite à partir du commit `b070e63b1`.
status: selected
draft: true
featured: false
edition: 2026-W48
notions:
  - Git
---
L'équipe de Godot publie la quatrième beta de la version 4.8, un peu plus d'une semaine après la beta 3. La majorité des bloquants restants a déjà été traitée. Cette version sera donc l'une des dernières beta du cycle, peut-être même la dernière. Les testeurs sont invités à signaler dès maintenant les bugs et régressions qu'ils trouvent. La version est disponible pour Linux, macOS et Windows, en édition standard et .NET. Elle existe aussi en éditeur Web, en éditeur XR et en éditeur Android, ce dernier étant réservé à un groupe de test. L'édition .NET demande désormais .NET 10.0 ou plus, au lieu de .NET 8.

Les changements depuis la beta 3 corrigent pour l'essentiel des régressions. En 3D, les traînées (trails) ne mettent plus leurs points à une mauvaise échelle. Côté rendu, `VoxelGI` ne déborde plus de bornes non uniformes. Les lectures audio démarrent en pause quand le nœud lecteur ne peut pas être traité. Dans le cœur du moteur, la version du format texte change pour les scènes sauvegardées qui contiennent des métadonnées ou des scènes intégrées. Dans l'éditeur, l'inspecteur se vide quand son objet est invalidé, et `CodeEditorBase` utilise la coloration syntaxique « Standard » au lieu de « Plain Text ». En GDScript, les soulignements de diagnostic sont décalés selon l'interligne pour ne plus chevaucher les lignes voisines.

L'import et les plateformes reçoivent aussi des corrections. L'extension `.mesh` apparaît de nouveau parmi les formats d'enregistrement de la fenêtre « Advanced Import Settings ». Les imports de polices dynamiques invalides sont maintenant validés et échouent. Sous Linux et Windows, le moteur avertit quand il ne trouve pas les symboles de débogage. L'export Web peut désormais servir en HTTPS. Sous Windows, des problèmes de configuration du gestionnaire de SDK Android sont corrigés. Côté système de build, la sortie de `--help` est alignée sur la page de manuel et sur les scripts de complétion du shell.

Au total, 58 contributeurs ont soumis 123 correctifs pour cette version, construite à partir du commit `b070e63b1`. L'illustration de couverture vient de JOHN GLEEP, un roguelike d'action où un petit extraterrestre enlève les habitants d'autres planètes pour en faire les clients de son casino et rembourser sa dette envers la mafia.
