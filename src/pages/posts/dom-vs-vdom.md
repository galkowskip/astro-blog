---
layout: ../../layouts/BlogLayout.astro

title: "DOM vs Virtual DOM"
pubDate: 2022-07-01
author: "Paweł Gałkowski"
image:
  url: "https://docs.astro.build/assets/full-logo-light.png"
  alt: "The full Astro logo."
tags: ["html", "frontend", "js", "react", "vue", "basics"]
---

## Czym jest DOM

DOM (Document Object Model) to drzewo reprezentujące strukturę HTML strony internetowej którą może manipulować Javascript. Jakakolwiek zmiana w DOM powoduje przeładowanie się całego drzewa przez co update są powolne i wymagające.

## Czym jest Virtual DOM

Virtual DOM to wirtualizacja struktury zwykłego DOM przechowywana w pamięci i używana do update'owania prawdziwego DOM. VDOM jest używany w frameworkach jak Vue, React czy Angular.

Gdy zmienia się coś w VDOM framework porównuje te zmiany z DOM i zmienia tylko te elementy które tego potrzebują dzięki czemu działania na VDOM są znacznie szybsze i wydajniejsze.

## Różnice pomiędzy DOM a VDOM

- DOM reprezentuje uproszczoną strukture HTML strony, VDOM uproszczone drzewo DOM.
- DOM może manipulować elementami HTML, VDOM może tylko robić update DOM.
- DOM kiedy się zmienia, całe drzewo DOM jest update'owane.
