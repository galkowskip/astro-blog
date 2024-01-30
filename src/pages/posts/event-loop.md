---
layout: ../../layouts/BlogLayout.astro

title: "Event Loop"
pubDate: 2022-07-01
author: "Paweł Gałkowski"
image:
  url: "https://docs.astro.build/assets/full-logo-light.png"
  alt: "The full Astro logo."
tags: ["js", "eventloop", "frontend", "asynchronous", "basics"]
---

## Loop

Event Loop to system w jakim uruchamiany jest JavaScript w Node i w przeglądarce. System jest podzielony na:

- Call Stack - stos FIFO eventów które mają być uruchomione. Kiedy dany item z zostanie wykonanny, jest usuwany ze stacku i wykonuje się następny w kolejce. W ten sposób wykonywany jest kod synchroniczny. Nie ma tu żadnych eventów `setTimeout` czy `Promise`. Kiedy nie ma niczego w tym stacku, nic się nie wykonuje a Javascript czeka aż cokolwiek się pojawi
- Callback Queue - stos z "eventami" i rzeczami które nie wykonujá się w kolejności kodu. Tutaj lądują wczelkie `callback`, I/O eventy, `Promise` i `setTimeout`/`setInterval`. Ten stos też jest stosem First in First out, który "działa" tylko wtedy kiedy "call stack" jest pusty. Itemy z tego stacka są przenoszone do Call Stacka i tam uruchamiane.

A więc kolejność wykonywania to:

- Wykonuj przedmioty z Call Stacka
- Jeżeli Call Stack pusty, przenieś do niego pierwszy item z Callback Queue
- Powtarzaj

## Podziały Callback Loop

Callback Loop dzieli się jeszcze na dwa stacki:

- Microtask Stack
- Macrotask Stack

Różnią się one typem eventów które do nich trafiają oraz kolejnością ich wykonywania. Microtask Stack jest opróżniany pierwszy, i dopiero kiedy on jest pusty przechodzimy do Macrotask Stacka

Microtaski to: `process.nextTick`, `Promises`, `queueMicrotask`, `MutationObserver`
