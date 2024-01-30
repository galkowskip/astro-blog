---
layout: ../../layouts/BlogLayout.astro

title: "TypeScript w Vue"
pubDate: 2022-07-01
author: "Paweł Gałkowski"
image:
  url: "https://docs.astro.build/assets/full-logo-light.png"
  alt: "The full Astro logo."
tags: ["js", "ts", "vue", "frontend"]
---

#vue #js #ts

## Ref()

[[Reactivity#Ref()]]

Ref potrafi inferować typy z przekazanej do niego wartości.

```js
const string = ref("hello world");

string.value = 0; // TS Error: Type 'number' is not assignable to type 'string'.
```

aby przekazać typ zmiennej do funkcji ref możemy zrobić to w dwa sposoby:

```js
const string: Ref<string> = ref("hello world");
```

lub

```js
const string = ref < string > "hello world";
```

## Reactive()

[[Reactivity#Reactive()]]

Reactive potrafi inferować typ przekazywanej do niego zmiennej.

```js
// type: { title: string }
const book = reactive({
  title: "Hello",
});
```

ale możemy też określić typ nowej zmiennej poprzez przekazanie go w następujący sposób:

```js
// type: { title: string }
const book: { title: string } = reactive({
  title: "Hello",
});
```
