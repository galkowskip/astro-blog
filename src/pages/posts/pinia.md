---
layout: ../../layouts/BlogLayout.astro

title: "Pinia"
pubDate: 2022-07-01
author: "Paweł Gałkowski"
image:
  url: "https://docs.astro.build/assets/full-logo-light.png"
  alt: "The full Astro logo."
tags: ["js", "vue", "ts", "frontend"]
---

To store library stworzona na potrzeby Vue 3. Jest bardzo podobna do Vuex'a z drobnymi różnicami.

- Uproszczone API - łatwiejsza i szybsza inicjalizacja store.
- Pozbycie się mutacji - od teraz stan można zmieniać już w akcjach pozbywając się jednego z kroków oraz małej duplikacji.
- Możliwość subskrybcji stanu i reagowania na jego zmiany.
- Większa modułowość - zamiast jednego store rozbitego na moduły mamy możliwość tworzenia wielu store'ów i podłączania pod komponenty tylko tych które dany komponent wymaga.
- Wsparcie TypeScripta
