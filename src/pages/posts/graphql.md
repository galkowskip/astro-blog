---
layout: ../../layouts/BlogLayout.astro

title: "GraphQL"
pubDate: 2022-07-01
author: "Paweł Gałkowski"
image:
  url: "https://docs.astro.build/assets/full-logo-light.png"
  alt: "The full Astro logo."
tags: ["js", "backend", "frontend", "asynchronous"]
---

GraphQL to język zapytań (query language) udostępniający wspólny interfejs między klientem a serwerem do manipulowania i pobierania danych. Jego użycie jest alternatywą dla zwykłego REST API.

Wszystkie requesty do GraphQL wysyłane są do jednego endpointa który zarządza query GraphQL'a np: `testsite.com/graphql` i oddaje JSON'a

#### vs REST

W odróżnieniu od API REST, GraphQL znacznie lepiej się skaluje, i wprowadza mniej chaosu związanego z różnymi ścieżkami i metodami pobierania danych.

Największym bonusem używania GraphQL w odróżnieniu od REST jest możlwiość pobierania za jednym requestem wszystkich danych jakie chcemy, i tylko tych które chcemy. Tam gdzie w REST musielibyśmy odpytywać kilka różnych ścieżek aby dostać dane które chcemy, lub budować specjalne route'sy na serwerze które podadzą nam potrzebne dane, w GraphQL wystaczy poprawnie zbudowane query aby dostać te dane, i tylko te dane które chcemy.

### Struktura Query

### Apollo
