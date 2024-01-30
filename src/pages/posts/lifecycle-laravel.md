---
layout: ../../layouts/BlogLayout.astro

title: "Lifecycle Aplikacji Laravel"
pubDate: 2022-07-01
author: "Paweł Gałkowski"
image:
  url: "https://docs.astro.build/assets/full-logo-light.png"
  alt: "The full Astro logo."
tags: ["laravel", "php", "backend"]
---

## public/index.php

Serwer na którym ustawiona jest aplikacja laravel powinien być zdefiniowany tak aby każdy request przez niego obsługiwany był kierowany do ścieżki `/` aby móc być obsłużony przez plik wejściowy aplikacji Laravel, czyli właśnie `public/index.php` którego zadaniem jest załadowanie reszty aplikacji i zależnosci zdefiniowanych przez Composer.

Następnym krokiem jest załadowanie instancji aplikacji Laravel z pliku `bootstrap/app.php`.

## Kernel HTTP/Console

W zależności od typu requesta kierowany jest dalej do kerneli HTTP lub Console.

Kernel HTTP tworzy środowisko do obsługi requestu, wykonuje `bootstrappery` configuruje logger itp. W nim też rejestrowane są globalne `Middleware`. Klasa Kernela ma też metode `handle` która otrzymuje atrybut `Request` i zwraca `Response`.

## Service Providers
