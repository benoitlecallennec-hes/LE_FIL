---
title: "Agent Host Protocol : le protocole ouvert qui permet de reprendre une session d'agent Copilot d'un appareil à l'autre"
date: 2026-11-17T18:20:40.000Z
source: The GitHub Blog
link: https://github.blog/engineering/engineering-principles/building-the-agent-host-protocol-for-sessions-that-continue-across-devices/
tags:
  - autres
feed_tag: Engineering
lang: en
level: intermediate
format: article
reading_time: 8
image: https://github.blog/wp-content/uploads/2026/11/ahp-protocol-state.png?resize=1024%2C512
summary: Chez GitHub, Connor Peet et Rob Lourens présentent l'Agent Host Protocol (AHP). Ce standard ouvert, né dans VS Code, permet de lancer une session d'agent Copilot sur un appareil et de la reprendre sur un autre. Il s'organise en canaux identifiés par URI, dont l'état est géré par des reducers.
takeaways:
  - "AHP est un standard ouvert de GitHub et VS Code pour reprendre une session d'agent d'un client à l'autre : éditeur, application, téléphone."
  - Le protocole s'organise en canaux identifiés par URI. Les canaux `root`, `session` et `chat` sont obligatoires, et l'état de chaque canal est géré par un reducer, avec le serveur comme arbitre.
  - "Il existe deux implémentations : un hôte en TypeScript pour VS Code et un hôte en Rust pour le cloud GitHub. Des bibliothèques sont disponibles en six langages."
  - Dans VS Code, `code agent host` lance un hôte autonome et `code agent endpoints` en affiche les points d'accès.
status: rejected
draft: true
featured: false
---
Au départ, GitHub Copilot était une extension de VS Code qui embarquait son propre harnais. La vie de l'agent dépendait donc de celle de l'éditeur. Fermer la fenêtre arrêtait l'agent, une mise à jour de VS Code arrêtait tous les agents, et une connexion à une machine distante créait un nouvel hôte d'extensions, verrouillé sur la version du workbench. De leur côté, l'application Copilot et les applications mobiles GitHub intégraient chacune les agents cloud à leur manière. Pour sortir de ces intégrations au cas par cas, les auteurs proposent un protocole versionné indépendamment : chaque client y voit les mêmes agents, le même historique et le même état.

AHP doit fonctionner avec plusieurs clients sur les mêmes agents, résister aux coupures réseau, aux reconnexions et aux modifications simultanées de plusieurs clients. Contrairement au Language Server Protocol ou au Debug Adapter Protocol, il ne décrit pas le fonctionnement des agents, un domaine qui évolue vite. Il décrit seulement la façon d'interagir avec eux. Le protocole s'organise en « canaux » identifiés par URI. Tout serveur doit implémenter les canaux `root`, `session` et `chat`, et peut en ajouter d'autres : terminal, compléments MCP, rapports OTel. Chaque canal porte un objet d'état géré par un reducer. Le serveur fait autorité sur cet état. Les clients envoient des actions (`dispatchAction`) qui modifient l'état ou déclenchent des effets de bord, comme l'envoi d'un message de chat. Après une coupure, un client renvoie les actions mises en file d'attente, affiche un état de chargement et ne valide chaque action qu'une fois qu'elle est réconciliée et acquittée par le serveur. Ce modèle facilite aussi le débogage et s'accorde avec des frameworks d'interface comme React.

Côté VS Code, il a fallu écrire un nouveau harnais. L'extension Copilot Chat comptait plus de 400 000 lignes de TypeScript, liées à l'API d'extensions et à la durée de vie d'une fenêtre. Le nouvel hôte repose sur le Copilot SDK et atteint environ 369 000 lignes de TypeScript. L'hôte de GitHub, qui doit tourner dans le cloud, est écrit en Rust et compte environ 305 000 lignes. Comme MCP, AHP prévoit des méthodes personnalisées et des données `_meta` pour faire passer des informations hors protocole. En faisant dialoguer les deux hôtes et leurs clients, l'équipe a découvert des manques dans le protocole, par exemple une sortie d'outil de type shell sans PTY, et des hypothèses fausses, comme un client VS Code qui supposait une structure fixe pour les URI de session.

Des bibliothèques AHP existent en Rust, TypeScript, Kotlin, Swift, .NET et Go. Dans l'application Copilot, on ajoute un hôte AHP en activant Experimental > Direct Connections. Dans VS Code, c'est l'option Add Remote Agent Host de la fenêtre Agents. La commande `code agent host` lance un hôte autonome, et `code agent endpoints` affiche les points d'accès de l'hôte de l'éditeur.
