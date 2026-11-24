---
title: Simon Willison harmonise l'apparence des listes `<select>` dans Chrome, Safari et Firefox
date: 2026-11-22T19:45:06.000Z
source: Simon Willison
link: https://simonwillison.net/2026/Nov/22/select-styling/
tags:
  - autres
feed_tag: tools
lang: en
level: beginner
format: article
reading_time: 1
image: https://static.simonwillison.net/static/2026/select-firefox-safari-chrome.webp
summary: Gêné par l'aspect des `<select>`, qui change d'un navigateur à l'autre et qu'il trouve laid dans Chrome, Simon Willison a fait générer par Claude une page qui recense les façons de les styler. Il en a tiré une règle CSS qu'il préfère désormais.
takeaways:
  - Sans style, un `<select>` n'a ni la même couleur, ni la même bordure, ni la même taille dans Chrome, Safari et Firefox.
  - "`appearance: none` supprime le rendu natif de la liste déroulante."
  - Une flèche SVG intégrée dans une URL `data:` et placée avec `background` remplace l'indicateur d'origine.
  - La page « Select Styling Specimens », générée avec Claude Fable 5.2, recense les façons de styler un `<select>`.
status: selected
draft: true
featured: false
edition: 2026-W48
---
Par défaut, une liste déroulante HTML `<select>` n'a pas le même aspect partout. Dans Chrome, elle est blanche avec une bordure noire. Dans Safari, elle est grise et sans bordure. Dans Firefox, elle est grise avec une bordure noire. Les trois n'ont pas tout à fait la même taille, et Simon Willison trouve la version de Chrome plutôt laide.

Il a donc demandé à Claude Fable 5.2 de construire une page, « Select Styling Specimens », qui montre toutes les façons de styler un `<select>`. Il voulait la photographier dans chaque navigateur, sur ordinateur comme sur mobile. Après quelques essais, il a retenu la règle ci-dessous. `appearance: none` supprime le rendu natif. Une petite flèche SVG, intégrée dans une URL `data:`, remplace l'indicateur d'origine. Elle est placée à droite grâce à la propriété raccourcie `background`.

```css
select {
  appearance: none;
  color: #1f2933;
  padding: 0.5em 2.25em 0.5em 0.75em;
  border: 1px solid #c4cbd3;
  border-radius: 6px;
  background: #fff url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 12 8'%3E%3Cpath d='M1 1.5l5 5 5-5' fill='none' stroke='%23667085' stroke-width='1.5'/%3E%3C/svg%3E") no-repeat right 0.75em center / 0.75em;
}
```

Avec cette règle, la liste a le même aspect dans Chrome, Safari et Firefox : une bordure grise aux coins arrondis et une flèche grise pointant vers le bas.
