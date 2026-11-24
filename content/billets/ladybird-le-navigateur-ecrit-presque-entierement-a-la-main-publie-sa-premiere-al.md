---
title: Ladybird, le navigateur écrit presque entièrement à la main, publie sa première alpha
date: 2026-11-23T21:46:35.000Z
source: Simon Willison
link: https://simonwillison.net/2026/Nov/23/ladybird/
tags:
  - ia
feed_tag: ai
lang: en
level: beginner
format: article
reading_time: 4
image: https://static.simonwillison.net/static/2026/ladybird-ffmpeg.webp
summary: Ladybird publie sa première alpha, installable sur macOS et Linux sans compilation. Simon Willison l'a essayé et revient sur le mois d'octobre, où des agents de code ont fait gagner au projet 75 650 tests `web-platform-tests`.
takeaways:
  - Première alpha de Ladybird, installable sur macOS et Linux sans compilation ; ses composants essentiels sont maison, sauf HarfBuzz, Skia, ffmpeg et SQLite.
  - Les applications WebAssembly (ffmpeg, Pyodide) fonctionnent, mais Datasette Lite bute sur `navigator.serviceWorker.startMessages`, pas encore implémenté.
  - En octobre, des agents de code ont ajouté environ 60 000 réussites aux `web-platform-tests` en cinq jours ; Ladybird y dépasse Safari.
  - "Aucune garantie de sécurité pour l'instant : à réserver à une navigation sans compte sensible."
status: rejected
draft: true
featured: false
---
Ladybird est un nouveau navigateur qui implémente lui-même l'essentiel de ses composants : analyse HTML, moteur CSS, mise en page, JavaScript et WebAssembly. Il ne s'appuie que sur quelques bibliothèques externes : HarfBuzz pour la mise en forme du texte, Skia pour le dessin 2D, ffmpeg pour la vidéo et SQLite. Le reste a été écrit pour Ladybird ou vient de SerenityOS, le projet dont il est issu. La première alpha peut s'installer sur macOS ou Linux sans compiler le code. Simon Willison le trouve rapide, avec une interface légère et intuitive, et la plupart des sites qu'il a visités s'affichent correctement. Chaque onglet tourne dans son propre processus `WebContent`, aux côtés de processus comme `RequestServer`, `Compositor`, `MediaServer` et `VideoDecoderServer`.

Ses outils les plus exigeants fonctionnent : un compresseur vidéo qui exécute ffmpeg compilé en WebAssembly, un outil de redimensionnement d'images et un REPL Pyodide (Python compilé en WebAssembly). Datasette Lite fait exception. Il combine des service workers et des web workers, et son démarrage échoue avec l'erreur `undefined is not a function (evaluated from 'navigator.serviceWorker.startMessages')`. Willison a ouvert un ticket, puis trouvé une annotation `FIXME` sur ce point dans le code source.

Cette année, le projet s'appuie beaucoup sur les agents de code. En février 2026, Andreas Kling a décrit le portage d'une partie du moteur LibJS du C++ vers 25 000 lignes de Rust en deux semaines, avec Claude Code et Codex. En octobre, le style et la mise en page ont été déplacés sur leur propre thread et un nouveau JIT JavaScript est arrivé. Un nouveau moteur WebAssembly a aussi été multiplié par 7 sur le benchmark WASM de JetStream 3. Sur les quelque 2,25 millions de sous-tests de `web-platform-tests`, Ladybird a gagné 75 650 réussites et dépasse désormais Safari. L'essentiel de ce gain a été obtenu en cinq jours, du 22 au 27 octobre, en demandant à des agents de compléter les fonctionnalités manquantes. Les plus gros progrès concernent `/css` (environ 30 000) et `/editing` (environ 10 000). Willison ne sait pas quels modèles ont été utilisés.

Andreas Kling prévient que, l'alpha étant une alpha, certains sites ne fonctionneront pas et le navigateur plantera parfois. Le projet ne fait encore aucune promesse de sécurité et invite les chercheurs à attaquer le navigateur et à signaler les failles via `SECURITY.md`. Willison ne s'y connectera donc pas à ses comptes sensibles (GitHub, Gmail). Il l'utilisera pour naviguer anonymement, en partie pour aider aux tests, et parce qu'un nouvel arrivant sur le marché des moteurs de navigateur le rend optimiste.
