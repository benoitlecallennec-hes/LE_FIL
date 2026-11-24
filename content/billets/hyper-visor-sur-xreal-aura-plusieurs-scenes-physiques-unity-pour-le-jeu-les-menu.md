---
title: "Hyper Visor sur XREAL AURA : plusieurs scènes physiques Unity pour le jeu, les menus et la pause"
date: 2026-11-23T00:00:00.000Z
source: Unity Blog
link: https://unity.com/blog/hyper-visor-xreal-aura-physics-scenes
tags:
  - 3d
lang: en
level: intermediate
format: article
image: https://mip.web.unity.com/img/mTNGYl9nG3YM5OXGWYzGqaWNcERCiffDv_bYONZOoMU/w:833/h:500/r:96,0,833,500/q:75/f:avif/aHR0cHM6Ly9jZG4uc2FuaXR5LmlvL2ltYWdlcy9mdXZiampscC9wcm9kdWN0aW9uL2UyMTQwZTBlYTEyYzUwZjVlMzE0M2VkYzFkZTg4MWM5ZDFlNTI2ZjUtMTAyNHg1MDAucG5n.avif
summary: Owlchemy Labs explique comment Hyper Visor donne à chaque « monde » sa propre scène physique Unity. Un seul rayon d'énergie, guidé par la main, sert à la fois au jeu, aux menus et à la pause sur les lunettes XREAL AURA.
takeaways:
  - Chaque monde (joueur, gameplay, pause) a sa propre scène physique, simulée par `PhysicsScene.Simulate()` avec sa propre horloge.
  - On crée les objets avec `_world.Spawn()` plutôt qu'`Instantiate()`, et sans état global la pause s'ajoute facilement.
  - Le stencil buffer, avec un numéro par monde, affiche chaque monde à travers sa « déchirure ».
  - "Sur l'écran additif d'AURA, le sombre devient transparent : il faut des visuels clairs et saturés."
missing:
  - image principale (og:image) non confirmée
  - temps de lecture
status: selected
draft: true
featured: false
edition: 2026-W48
---
Hyper Visor est un jeu d'arcade XR d'Owlchemy Labs pour les appareils Android XR, développé avec Unity. Le joueur suit ses mains et tire un rayon d'énergie sur des cibles, à travers huit niveaux avec des boss. Le rayon part du poignet quand la main est ouverte et s'arrête quand elle se ferme. Sa direction ne dépend que de la tête et du poignet. Toutes les interactions passent par la physique : chaque cible a un `Rigidbody` que le rayon touche par sphere cast. Le jeu n'a pas de système d'interface séparé. Les menus et les réglages utilisent les mêmes cibles flottantes, avec un anneau de vie qui se vide quand on vise la cible.

Le jeu est découpé en mondes : le joueur, le gameplay et le menu de pause. Chaque monde a sa propre scène physique, et le handle de cette scène sert de clé pour le retrouver depuis `gameObject.scene` dans `Awake()`. Les systèmes appellent `_world.Spawn()` au lieu d'`Instantiate()` et demandent l'heure à l'horloge de leur monde. Un `GameModule` coordonne les mondes, qui ne communiquent pas entre eux. Dans son `FixedUpdate()`, il fait tourner la logique de jeu, puis appelle `PhysicsScene.Simulate()` pour chaque monde avec son horloge. L'équipe décide elle-même quand appeler `Physics.SyncTransforms()`, et `Time.timeScale` ne sert qu'aux changements de focus de l'appareil. Pour mettre en pause, on détruit une cible dans le monde de la pause. Le jeu n'avait aucun état global, si bien que la pause, ajoutée tard, a été facile à intégrer et la reprise se fait sans saccade.

L'affichage des mondes s'appuie sur le stencil buffer : chaque monde a son numéro de stencil, et chaque shader a un bloc de propriétés stencil pour dessiner le contenu à l'intérieur des « déchirures de la réalité ». Les scènes physiques restent petites, une centaine de `Rigidbody` à la fois, sans géométrie statique. Le temps applicatif reste stable autour de 7 ms sur AURA. Pour ne pas recompiler à chaque essai, ce qui pouvait prendre 40 secondes, les niveaux sont des Prefabs qu'on recharge avec la touche R. Leur déroulement est écrit dans un langage de script maison, lu au moment de l'exécution :

```text
points SpawnPoints Top1,Top2,Top3 cycle_after:1
points VirusSpawnPoints Bottom1,Bottom2,Bottom3 cycle_after:1

shuffle SpawnPoints

loop_until objective_met

spawn Network point:SpawnPoints count:6 interval:0.4
wait 1
spawn Virus point:VirusSpawnPoints count:3 interval:0.5 rotate:45
wait 1

end
```

L'écran additif d'AURA rend les pixels sombres transparents. Les cibles et le personnage ont donc été repensés avec des couleurs plus claires et plus saturées. La plupart des développeurs travaillaient sur Meta Quest 3, avec une Renderer Feature URP qui recadre l'image au format d'AURA. Le développement s'est fait sous Unity 6.2, et la version de lancement est sortie sous Unity 6.5, par-dessus le plug-in OpenXR.
