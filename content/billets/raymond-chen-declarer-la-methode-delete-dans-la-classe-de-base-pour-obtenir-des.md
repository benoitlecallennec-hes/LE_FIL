---
title: "Raymond Chen : déclarer la méthode `= delete` dans la classe de base pour obtenir des erreurs de compilation plus claires"
date: 2026-11-20T21:45:56.000Z
source: isocpp.org
link: https://isocpp.org//blog/2026/11/on-forcing-all-derived-classes-to-implement-a-specific-non-virtual-met
tags:
  - cpp
lang: en
level: intermediate
format: article
reading_time: 3
image: https://isocpp.org/files/img/RaymondChen_5in-150x150.jpg
summary: Pour obliger chaque classe dérivée à implémenter une méthode non virtuelle, Raymond Chen propose de la déclarer `= delete` dans la classe de base. Le message d'erreur du compilateur désigne alors directement la méthode à écrire, et C++26 permet d'y ajouter une explication.
takeaways:
  - Déclarer la méthode `= delete` dans la classe de base montre au développeur la signature à implémenter et place l'erreur à côté d'un commentaire explicatif.
  - C++26 accepte `= delete("message")`. Clang et GCC affichent ce message dans l'erreur, MSVC ne le gère pas encore.
  - La technique échoue si la méthode redéfinit une déclaration d'une classe de base, comme en C++/WRL. Elle fonctionne en C++/WinRT et en C++ standard.
status: published
featured: false
edition: 2026-W48
---
Dans la première partie, Raymond Chen montrait une façon simple d'obliger les classes dérivées à fournir une méthode non virtuelle : ne pas l'implémenter dans la classe de base et attendre que la compilation échoue. Le compilateur signale bien le problème, mais l'explication se trouve souvent dans les « informations d'erreur supplémentaires ». Le développeur ne sait pas toujours les retrouver, ni comment corriger l'erreur une fois qu'il les a lues. Chen propose donc de déclarer la méthode dans la classe de base en la marquant `= delete`. Il illustre la technique en C++/WRL, en C++/WinRT et en C++ standard avec un `OneWayConverter` : `Convert()` y est supprimée, tandis que `ConvertBack()` lève une exception « non implémenté ».

Cette approche a deux avantages. D'abord, le développeur voit la signature exacte à implémenter, puisque c'est celle qui est supprimée dans la classe de base. Ensuite, l'erreur renvoie à la déclaration supprimée, et un commentaire placé à cet endroit peut expliquer ce qu'il faut faire. Avec MSVC, on obtient l'erreur C2280 (« attempting to reference a deleted function »), suivie d'un renvoi à la déclaration de `OneWayConverter::Convert`.

C++26 permet d'associer un message à une fonction supprimée. MSVC ne le gère pas encore. Clang et GCC le prennent en charge et affichent ce message dans le texte principal de l'erreur, par exemple `error: attempt to use a deleted function: If you derive from OneWayConverter, you must implement Convert()`.

```cpp
winrt::Windows::Foundation::IInspectable
    Convert(
        winrt::Windows::Foundation::IInspectable const& /*value*/,
        winrt::Windows::UI::Xaml::Interop::TypeName const& /*targetType*/,
        winrt::Windows::Foundation::IInspectable const& /*parameter*/,
        winrt::hstring const& /*language*/)
    = delete("If you derive from OneWayConverter, you must implement Convert()");
```

La technique fonctionne en C++/WinRT et en C++ standard, mais pas en C++/WRL. En WRL, la classe dérive d'une classe de base abstraite, et on ne peut pas supprimer une méthode déclarée dans une classe de base. L'astuce ne s'applique donc que si la déclaration ne redéfinit pas une méthode d'une classe parente. Chen qualifie cette démarche de « métaprogrammation des messages d'erreur du compilateur » : selon lui, c'est un aspect sous-estimé de l'écriture d'une bibliothèque.
