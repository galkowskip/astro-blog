---
layout: ../../layouts/BlogLayout.astro

title: "Jak testować Vue"
pubDate: 2022-07-01
author: "Paweł Gałkowski"
image:
  url: "https://docs.astro.build/assets/full-logo-light.png"
  alt: "The full Astro logo."
tags: ["js", "frontend"]
---

Testować aplikacje Vue można na 3 sposoby:

- Unit Testing
- Component Testing
- End to End Testing

# Unit Testing

Są pisane do testowania małych odizolowanych części kodu, często np. do funkcji helperów.
Mogą mockować dużą część aplikacji, ale w najłatwiejszy sposób można w nich sprawdzić logikę funkcji i jej poprawność.
