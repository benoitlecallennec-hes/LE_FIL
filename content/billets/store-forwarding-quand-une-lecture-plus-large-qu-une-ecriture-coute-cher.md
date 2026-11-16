---
title: "Store forwarding : quand une lecture plus large qu'une écriture coûte cher"
date: 2026-11-05T13:06:20.000Z
source: lemire.me
link: https://lemire.me/blog/2026/11/05/a-performance-trap-when-store-forwarding-fails/
tags:
  - autres
lang: en
level: advanced
format: article
reading_time: 3
image: https://lemire.me/blog/wp-content/uploads/2026/11/storeforwarding-feature-150x150.jpg
summary: Daniel Lemire montre que relire sur 8 octets une donnée tout juste écrite sur 1 ou 4 octets fait échouer le *store forwarding*. Sur x86, cette relecture coûte de 14 à 19 cycles au lieu d'un seul.
takeaways:
  - Le *store forwarding* échoue quand la lecture est plus large que l'écriture qui la précède ou ne la recouvre qu'en partie, par exemple `memcpy(p, &v, 1)` suivi de `memcpy(&w, p, 8)`.
  - Sur les Xeon et l'EPYC, cet échec fait passer la relecture de 1 cycle à 14–19 cycles. Sur Apple M4 et Graviton 5, elle coûte 5 cycles, comme une relecture normale.
  - Pour l'éviter, modifier la valeur dans un registre (`(x & ~0xffull) | v`) puis l'écrire en une fois, à la même taille que les lectures qui suivent.
status: published
featured: false
edition: 2026-W47
---
Une écriture en mémoire ne va pas directement dans le cache : l'attendre bloquerait le processeur. Elle passe d'abord par un *store buffer*, et une lecture qui suit peut y prendre ses octets. C'est le *store forwarding*. Il fonctionne quand la lecture a la même taille que l'écriture, ou quand elle est plus petite et entièrement contenue dedans. Il échoue quand la lecture est plus large que l'écriture ou quand les deux plages ne se recouvrent qu'en partie. La lecture doit alors attendre que l'écriture ait atteint le cache.

Lemire illustre le piège en C : on écrit un seul octet dans un tampon avec `memcpy`, puis on relit 8 octets à la même adresse.

```c
uint64_t reload_byte(char *p, unsigned char v) {
  // Copy one byte from v into the buffer.
  memcpy(p, &v, 1);
  uint64_t w;
  // Copy eight bytes from the buffer into w.
  memcpy(&w, p, 8);
  return w;
}
```

Une écriture de 4 octets suivie d'une lecture de 8 pose le même problème. C'est aussi le cas d'une écriture de 8 octets suivie d'une lecture de 8 octets qui commence 4 octets plus loin, puisque les plages se chevauchent. Pour éviter le piège, on peut faire la modification dans un registre : `update_byte` charge les 8 octets dans un `uint64_t` et remplace l'octet de poids faible avec `(x & ~0xffull) | v`. Elle réécrit ensuite les 8 octets d'un seul bloc. Si l'on écrit 8 octets puis qu'on relit exactement les mêmes, le compilateur peut même supprimer la relecture.

Lemire a mesuré ces cas sur cinq processeurs : Xeon Gold 6548N, Xeon 6975P-C, EPYC 9R45, Graviton 5 et Apple M4 Max. Sur les Xeon et l'EPYC, une lecture de même taille que l'écriture prend 1 cycle. Une lecture plus large en prend 19 sur les Xeon et 14 sur l'EPYC, et 20 et 15 après deux écritures de 4 octets. Sur le M4 et le Graviton 5, une lecture plus large coûte 5 cycles, ce qui correspond à leur latence normale entre une écriture et une lecture. Seules les deux écritures successives font monter le M4 à 8 cycles. Selon Lemire, le problème concerne donc surtout les processeurs x64. Le code source des mesures est disponible sur GitHub.
