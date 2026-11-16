---
title: JetBrains renouvelle son parrainage Platinum de la Godot Foundation pour 2027
date: 2026-11-10T12:00:00.000Z
source: godotengine.org
link: https://godotengine.org/article/partners_jetbrains_sponsorship_renewal_2027/
tags:
  - 3d
feed_tag: News
lang: en
level: beginner
format: article
reading_time: 3
image: https://godotengine.org/storage/blog/covers/partners_jetbrains_sponsorship_renewal_2027.jpg
summary: JetBrains reste sponsor Platinum de la Godot Foundation en 2027. L'annonce fait le point sur Rider, qui gère désormais GDScript, C# et les GDExtensions C++ dans un seul IDE.
takeaways:
  - JetBrains reste sponsor Platinum de la Godot Foundation en 2027, après une première année commencée en août 2025.
  - Rider gère GDScript, C# et les GDExtensions C++ (compilées avec CMake) dans un seul IDE, gratuit pour un usage non commercial.
  - Rider 2026.3, en accès anticipé, ajoute la navigation depuis $Node/%UniqueNode et l'arbre de scène distant pendant le débogage C#.
  - Une contribution de JetBrains au moteur a étendu le Debug Adapter Protocol de Godot, et d'autres outils peuvent en profiter.
status: rejected
draft: true
featured: false
---
La Godot Foundation annonce que JetBrains renouvelle son parrainage de niveau Platinum pour 2027. L'éditeur était devenu sponsor Platinum en août 2025. En parallèle, il a continué d'étendre la prise en charge de Godot dans Rider. En un an, l'IDE est arrivé à couvrir GDScript, C# et les GDExtensions en C++. Il est gratuit pour un usage non commercial : projets personnels, apprentissage, open source et création de contenu.

Pour GDScript, Rider propose la complétion de code, Find Usages, le renommage (refactoring), des inspections et un formateur. On peut aussi glisser des nœuds depuis l'aperçu de scène vers un script pour insérer leur chemin ou générer des variables @onready et @export. Côté débogage, Rider débogue GDScript et C#, y compris des scènes précises et des tool scripts, et peut déboguer les deux langages en même temps dans un projet mixte. Les tests C# s'exécutent avec le runner GoDotTest de Chickensoft. Pour le code natif, les GDExtensions C++ se compilent avec CMake. Sous Linux et macOS, le fichier godot-cpp.natvis permet d'inspecter les types natifs de Godot dans le débogueur. Des modèles de projet existent aussi pour les jeux, les assets et les plugins d'éditeur destinés à l'Asset Store de Godot.

Rider 2026.3, déjà disponible dans le programme d'accès anticipé (Early Access Program), ajoutera trois nouveautés. Il permettra de naviguer depuis les références $Node et %UniqueNode vers les nœuds de scène. Find Usages trouvera aussi les connexions de signaux faites via Callable. Enfin, l'arbre de scène distant et l'inspecteur de nœuds seront disponibles pendant le débogage C#. Un billet du blog JetBrains détaille l'ensemble, avec l'exemple de Mega Crit, qui développe Slay the Spire 2 avec Rider.

JetBrains contribue aussi directement au moteur. Ivan Šachov, développeur de Rider, a écrit une contribution fusionnée en octobre 2025. Elle étend la prise en charge du Debug Adapter Protocol de Godot avec la sélection de scène et les arguments de ligne de commande, que d'autres outils externes peuvent aussi utiliser. L'addon officiel JetBrains Rider Integration, disponible sur l'Asset Store, permet de configurer Rider comme éditeur externe de Godot.
