---
title: "`[[nodiscard]]` mal placé : MSVC l'ignore sans prévenir"
date: 2026-11-17T21:50:10.000Z
source: isocpp.org
link: https://isocpp.org//blog/2026/11/where-to-put-nodiscard-a-silent-trap-with-msvc-giovanni-dicanio
tags:
  - cpp
lang: en
level: intermediate
format: article
reading_time: 2
image: https://giodicanio.com/wp-content/uploads/2026/10/cpp_nodiscard_clang_error.png
summary: Giovanni Dicanio montre que Visual Studio 2022 accepte sans le moindre avertissement un `[[nodiscard]]` écrit après `static`, même en `/W4`. Le développeur se croit protégé alors que l'attribut ne sert à rien.
takeaways:
  - Avec MSVC 2022, `static [[nodiscard]] int f()` compile sans avertissement, même en `/W4`, et l'attribut ne sert à rien.
  - Écrire `[[nodiscard]] static int f()` déclenche bien l'avertissement C4834 quand la valeur de retour est ignorée.
  - Clang et GCC rejettent l'attribut placé après `static`.
  - Selon l'auteur, Visual Studio 2026 signale l'attribut ignoré avec l'avertissement C5240.
status: published
featured: false
edition: 2026-W48
---
Giovanni Dicanio déclare une fonction `GetSomething()` qui renvoie 64, avec l'attribut `[[nodiscard]]` placé après le mot-clé `static`. Il l'appelle ensuite depuis `main()` sans utiliser la valeur de retour. Il compile en mode C++20 avec Visual Studio 2022 et le niveau d'avertissement `/W4`, celui de la plupart de ses projets VC++. Il attend un avertissement et n'en obtient aucun : le code compile en silence.

```cpp
static [[nodiscard]] int GetSomething()
{
    return 64;
}

int main()
{
    GetSomething();
}
```

Il suffit de placer `[[nodiscard]]` en tête de la déclaration, avant `static`, pour que MSVC émette l'avertissement attendu, C4834 (« discarding return value of function with [[nodiscard]] attribute »). Clang et GCC, eux, refusent l'attribut à la mauvaise position : l'auteur montre l'erreur produite par Clang.

Pour l'auteur, le problème vient de ce silence. Il donne un « faux sentiment de sécurité » : on croit la fonction protégée contre l'oubli de sa valeur de retour, alors qu'elle ne l'est pas. Il demande que MSVC soit corrigé au plus vite. Dans un commentaire, il précise que Visual Studio 2026 en `/W4` signale désormais un attribut ignoré à cette position (avertissement C5240), alors que Visual Studio 2022 reste muet.
