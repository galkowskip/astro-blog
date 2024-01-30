---
layout: ../../layouts/BlogLayout.astro

title: "Vue Async Components"
pubDate: 2022-07-01
author: "Paweł Gałkowski"
image:
  url: "https://docs.astro.build/assets/full-logo-light.png"
  alt: "The full Astro logo."
tags: ["js", "ts", "frontend", "vue", "asynchronous"]
---

Komponenty Asynchroniczne w Vue są ładowane z serwera i renderowane jak sama nazwa wskazuje - asynchronicznie. Nie są więc w paczce z resztą plików i kodem JS, są ładowane dopiero wtedy kiedy sa potrzebne. Dzięki temu zmniejsza się początkowa ilość ładowanego kodu i skraca się czas renderowania aplikacji.

```js
const AsyncComponent = () => ({
  component: import("./AsyncComponent.vue"),
  loading: LoadingComponent,
  error: ErrorComponent,
  delay: 200,
  timeout: 3000,
});
```

Taki wrapper na komponent może być później wykorzystany aby zarejestrować komponent lokalnie lub globalnie i korzystać z niego jak z normalnego komponentu.
