---
title: "Vulkan Video arrive dans les navigateurs : décodage sans copie dans Firefox 153, encodeur matériel pour Chromium"
date: 2026-11-23T18:49:00.000Z
source: khronos.org
link: https://www.khronos.org/blog/vulkan-video-acceleration-comes-to-browsers
tags:
  - 3d
lang: en
level: intermediate
format: article
reading_time: 9
image: https://www.khronos.org/images/jcogs_img/cache/2026-blog-vulkan-video-acceleration-comes-to-the-browser_-_28de80_-_ade52cc9e2371411e2f6a5e18fb8664a9767175d.png
summary: Khronos présente Vulkan Video comme un backend unique d'accélération vidéo pour les navigateurs, valable sur tout GPU doté d'un pilote compatible. Firefox 153 intègre le décodage sans copie (zero-copy). Pour Chromium, l'encodeur est en cours d'intégration dans le projet et le décodage est en développement.
takeaways:
  - Vulkan Video ajoute à Vulkan des files de décodage (H.264, H.265, AV1, VP9) et d'encodage (H.264, H.265, AV1), avec un traitement sans copie, entièrement sur le GPU.
  - Firefox 153 décode sans copie grâce à FFmpeg, qui exporte les images en DMA-buf avec les modificateurs de format DRM et les barrières de décodage.
  - Dans Chromium, un seul encodeur Vulkan servira WebRTC, le partage d'écran, WebCodecs, MediaRecorder et Cast. Il est en cours d'intégration, et le décodage Vulkan est en développement.
  - Les pilotes NVIDIA, AMD et Intel prennent déjà Vulkan Video en charge, et les pilotes Mesa RADV, ANV et NVK suivent.
missing:
  - balise og:image non vérifiée (adresse de la bannière reprise)
status: published
featured: false
edition: 2026-W48
---
Tony Zlatinski préside le sous-groupe Vulkan Video de Khronos et travaille chez NVIDIA. Il part d'un constat : la vidéo est la charge la plus lourde du web, et c'est le plus souvent le navigateur qui la décode et l'encode. Or chaque plateforme a ses propres API : VA-API et V4L2 sous Linux, DXVA et Media Foundation sous Windows, des SDK propres à chaque fabricant dans l'embarqué. Les navigateurs doivent donc maintenir plusieurs backends, et certaines plateformes n'ont aucune accélération. Vulkan Video n'est pas une API à part. Il ajoute à Vulkan des files de décodage et d'encodage vidéo, qui reprennent la gestion de la mémoire, la synchronisation et l'ordonnancement déjà présents dans les pilotes. Des extensions propres à chaque codec complètent un socle commun : décodage H.264, H.265, AV1 et VP9, encodage H.264, H.265 et AV1. Les images restent sur le GPU du décodage jusqu'à l'affichage, et de la capture jusqu'à l'encodage. Les pilotes de NVIDIA, d'AMD et d'Intel le prennent déjà en charge. Les pilotes libres de Mesa (RADV, ANV, NVK) suivent.

Firefox décode les médias avec FFmpeg, dans un processus isolé (sandbox). Khronos a travaillé avec Mozilla et la communauté FFmpeg pour que le décodeur Vulkan Video de FFmpeg exporte les images décodées sous forme de DMA-buf, le mécanisme Linux de partage de mémoire GPU entre processus. Il exporte aussi les modificateurs de format DRM et les barrières de décodage dont l'autre processus a besoin. Le processus GPU de Firefox importe ces DMA-buf et les transmet directement au compositeur. Seul un handle d'image traverse la sandbox : les pixels ne passent jamais par la mémoire système. Le décodeur et la mise à jour des règles de sandbox, qui permet au processus média d'accéder au GPU via Vulkan, sont arrivés dans Firefox 153.

Sous Linux, Chromium encodait jusqu'ici toute la vidéo sur le CPU. Khronos a écrit un encodeur directement sur l'API Vulkan, à partir de la bibliothèque Vulkan Video Samples. Ce backend unique doit servir WebRTC, le partage d'écran, WebCodecs, MediaRecorder et Cast. Les images capturées arrivent sous forme d'images partagées GPU, sans copie, et l'encodeur est en cours d'intégration dans le projet Chromium. Côté décodage, Chromium s'appuie sur VA-API, que gèrent les pilotes Intel et AMD. NVIDIA a fourni sa propre implémentation de VA-API comme solution provisoire, en attendant le backend de décodage Vulkan Video, en développement.

Le projet a été coordonné par le sous-groupe Vulkan Video de Khronos, avec AMD, Intel, Google, Mozilla et la communauté FFmpeg. Il repose sur quatre couches : les spécifications et les tests de conformité de Khronos, les pilotes des fabricants de GPU, les bibliothèques (FFmpeg et Vulkan Video Samples), puis les navigateurs. Selon l'article, la même pile vaut aussi pour l'embarqué et l'automobile. Les utilisateurs de Firefox sous Linux sont invités à essayer le décodage Vulkan et à signaler sur Bugzilla les GPU et les pilotes qui fonctionnent.
