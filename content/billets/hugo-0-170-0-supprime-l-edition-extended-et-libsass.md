---
title: Hugo 0.170.0 supprime l'édition « extended » et LibSass
date: 2026-11-13T16:09:01.000Z
source: Hugo News
link: https://github.com/gohugoio/hugo/releases/tag/v0.170.0
tags:
  - autres
lang: en
level: intermediate
format: article
reading_time: 4
summary: "Avec la version 0.170.0, Hugo ne publie plus qu'un seul binaire : l'édition _extended_, compilée avec CGO, disparaît en même temps que LibSass. Dart Sass devient le seul transpileur Sass. La version apporte aussi plusieurs correctifs de sécurité et passe à Go 1.27.4."
takeaways:
  - "Hugo n'a plus qu'un binaire : l'édition _extended_ et LibSass sont supprimées, et Dart Sass est le seul transpileur"
  - Les archives `hugo_extended_*` ne sont plus publiées et `hugo.IsExtended` renvoie toujours `false`
  - Les options de la ligne de commande priment désormais sur les variables d'environnement comme `HUGO_ENVIRONMENT`
  - Le contenu _passthrough_ et `wrapperClass` sont maintenant échappés, et Hugo passe à Go 1.27.4
missing:
  - année de publication déduite du pied de page
status: selected
draft: true
featured: false
edition: 2026-W48
---
Le principal changement de Hugo 0.170.0 est la fin de l'édition _extended_. LibSass était obsolète depuis longtemps, et c'était la dernière raison de maintenir une version compilée avec CGO. Il ne reste donc qu'un seul binaire Hugo, qui offre toutes les fonctionnalités de l'ancienne édition _extended_, à l'exception de LibSass. Choisir `libsass` comme `transpiler` dans `css.Sass` provoque désormais une erreur : Dart Sass est le seul transpileur, et il faut l'installer pour utiliser Sass. Les archives `hugo_extended_*` et les builds `extended_withdeploy` ne sont plus publiés. Des alias sont conservés pour les archives les plus téléchargées, mais les notes demandent de mettre à jour les scripts. `hugo.IsExtended` renvoie toujours `false`, et le réglage de module `hugoVersion.extended` est déprécié et n'a plus d'effet.

D'autres comportements changent. Les options de la ligne de commande priment désormais sur les variables d'environnement. Cela corrige le cas de `HUGO_ENVIRONMENT=qa hugo build -e staging`, où `hugo.Environment` valait `qa` alors que la configuration `staging` était chargée. Les valeurs `time.Time` conservent leur fuseau horaire nommé, et `eq`, `ne`, `in` et `uniq` les comparent par instant. `resources.Match` respecte la casse des répertoires, ce qui fait fonctionner `resources.Match "gallery/Travel/Iceland/*"` sur les systèmes de fichiers sensibles à la casse. `strings.Truncate` est réécrit sur un véritable analyseur HTML : il gère les commentaires, les éléments vides et les éléments de texte brut comme `script`, et il est plus rapide. La synchronisation des fichiers statiques saute la vérification du contenu quand la taille et la date de modification sont identiques.

Côté sécurité, le contenu _passthrough_ est échappé en HTML quand aucun hook de rendu n'est défini, sauf si `markup.goldmark.renderer.unsafe` est activé. L'option `wrapperClass` du shortcode `highlight` et de `transform.Highlight` est maintenant échappée elle aussi. Les chemins des modules vendorisés sont limités au répertoire `_vendor`, et les liens symboliques qui en sortent sont rejetés. Hugo est compilé avec Go 1.27.4, qui apporte 12 correctifs de sécurité. Le plus important pour Hugo corrige un plantage du serveur HTTP/1 de `net/http`, utilisé par `hugo server`, causé par une situation de concurrence dans `MaxBytesReader`.

La version met aussi à jour plusieurs dépendances, dont `golang.org/x/net` 0.62.0, `github.com/yuin/goldmark` 1.8.7 et `github.com/go-jose/go-jose/v4` 4.1.6. Elle corrige plusieurs bogues : le tri dans `tpl/collections` quand certains éléments n'ont pas la clé de tri, la recherche de gabarits de shortcodes dans les formats de sortie non par défaut, et les traductions du projet qui perdaient face à celles des modules. Enfin, `Concat` accepte maintenant un `[]any` de ressources.
