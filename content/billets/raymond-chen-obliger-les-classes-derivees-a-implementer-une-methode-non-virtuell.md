---
title: "Raymond Chen : obliger les classes dérivées à implémenter une méthode non virtuelle, sans la déclarer dans la classe de base"
date: 2026-11-19T21:45:38.000Z
source: isocpp.org
link: https://isocpp.org//blog/2026/11/on-forcing-all-derived-classes-to-implement-a-specific-non-virtual-method-p
tags:
  - cpp
lang: en
level: intermediate
format: article
reading_time: 3
image: https://isocpp.org/files/img/RaymondChen_5in-150x150.jpg
summary: Pour qu'une classe dérivée fournisse une méthode obligatoire, comme `Convert()` d'un `IValueConverter`, il suffit de ne pas implémenter cette méthode dans la classe de base. Raymond Chen montre comment C++/WRL, C++/WinRT et le C++ standard transforment alors l'oubli en erreur de compilation.
takeaways:
  - Ne pas implémenter une méthode dans la classe de base suffit à obliger la classe dérivée à la fournir, sans `assert` ni exception à l'exécution.
  - C++/WRL signale l'oubli dès `WRL::Make` (classe abstraite, `C2259`), et C++/WinRT dès `winrt::make` (`C2039`, erreur typique du CRTP).
  - En C++ standard, l'erreur n'apparaît qu'au premier appel de `Convert`, ce que l'auteur promet d'améliorer dans la suite.
status: published
featured: false
edition: 2026-W48
---
Raymond Chen part d'un cas concret : une classe de base `OneWayConverter` implémente en partie l'interface `IValueConverter`. Son `ConvertBack()` échoue volontairement, puisque la conversion ne va que dans un sens, et c'est à la classe dérivée, ici `WidgetColorConverter`, de fournir `Convert()`. Lors d'une revue de code, la consigne n'était qu'un commentaire. Une première correction ajoutait dans la base une méthode `Convert` de secours, qui appelle `assert(false)` puis renvoie `E_NOTIMPL` (C++/WRL), lève `winrt::hresult_not_implemented()` (C++/WinRT) ou `std::exception("not implemented")` (C++ standard). Selon l'auteur, c'est « en faire trop ».

Sa solution est plus simple : pour forcer une classe dérivée à implémenter une méthode, on ne l'implémente pas du tout dans la classe de base. `OneWayConverter` ne garde que `ConvertBack()` et un commentaire.

```cpp
struct OneWayConverter
{
    // Derived classes must implement Convert()

    // One-way converters cannot convert back
    Widget ConvertBack(Color const& /*color*/)
    {
        throw std::exception("not implemented");
    }
};
```

Si la classe dérivée oublie `Convert`, chaque cas finit en erreur de compilation. Avec C++/WRL, l'appel à `WRL::Make` produit l'erreur classique `C2259` : impossible d'instancier une classe abstraite, car `IValueConverter::Convert` reste pure. Avec C++/WinRT, `winrt::make` déclenche une erreur `C2039` (« `Convert` n'est pas membre de `WidgetColorConverter` »), typique du CRTP : le modèle appelle `Convert` sur la classe dérivée, qui ne l'a pas. En C++ standard, la classe compile sans problème, et l'erreur `C2039` n'apparaît qu'au moment où quelqu'un appelle `Convert`. L'auteur ajoute qu'on peut faire mieux et annonce une suite.
