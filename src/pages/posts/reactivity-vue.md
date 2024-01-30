---
layout: ../../layouts/BlogLayout.astro

title: "Reactivity Vue"
pubDate: 2022-07-01
author: "Paweł Gałkowski"
image:
  url: "https://docs.astro.build/assets/full-logo-light.png"
  alt: "The full Astro logo."
tags: ["frontend", "vue", "ts", "vue"]
---

# Ref()

Funkcja `ref()` to polecany przez dokumentacje Vue sposób na. reaktywny stan komponentu.

Funkcja `ref()` zwraca opakowany w objekt ref argument. do niej podany. Objekt taki posiada reaktywną wartość w kluczu. `.value` która jest obserwowana i przy każdej zmianie tej wartości tworzony. jest nowy update w Vue.

```js
const count = ref(0);

console.log(count); // Object
console.log(count.value); // 0

count.value = 1;

await nextTick();
console.log(count.value); // 1
```

używając obiektów ref w template nie trzeba dodawać do nich `.value`, są automatycznie destrukturyzowane.

Obiekt ref można przekazać jako argument funkcji nie tracąc dostępu do jego reaktywności. Przekazujemy wtedy cały obiekt, nie tylko jego klucz `value`

Funkcja `ref()` akceptuje wszystkie typy zmiennych. I w odróżnieniu od reaktywności Vue 2, śledzi zmiany we wszystkich znajdujących się w nim obiektach i arrayach (jest "deeply reactive"). Jest w stanie śledzić te zmiany ponieważ przekształca przekształca wszystkie zagnieżdżone w pierwszym obiekcie zmienne nie-prymitywne (objekty, arraye itp.).

```js
const object = {
  object2: {
    val: 10,
  },
  array: [1, 2],
};

object.object2.val.value = 20;

console.log(object.object2.val.value); // 20
```

Śledzenie zmian w dużym obiekcie tego typu może powodować problemy z wydajnością. Można też zrezygnować z przekształcania wszyskich zagnieżdżeń w obiekty reaktywne poprzez zastosowanie `shallow ref`, w takim wypadku tylko pierwszy obiekt jest reaktywny.

[[Typescript w Vue#Ref()]]

# Reactive()

Drugim sposobem na reaktywność w Vue jest stosowanie `reactive` który nie wrappuje podanego do niego obiektu w specjalny obiekt, lecz sam robi z tego obiektu obiekt reaktywny.

Głównym minusem użycia `Reactive` jest to że przekazany argument do funkcji `reactive()` **musi być obiektem**, nie można go użyć na "prymitywach".

```jsx
const state = reactive({ count: 0 });
```

```html
<button @click="state.count++">{{ state.count }}</button>
```

Objekty w `reactive` korzystają z JavaScriptowego systemu `Proxy` dzięki czemu zachowują się jak zwykłe obiekty lecz są w stanie przechwytywać jego `gettery` i `settery` i reagować na nie reaktywnością.

Tak samo jak w `ref()` obiekty przekazane do `reactive` są "deeply reactive" czyli wszystkie zagnieżdżone w nich obiekty są także przekształcane w obiekty `reactive`. Podobnie jak w `ref` możemy też aby pominąć to przekształcenie użyć `shallowReactive()`.

[[Typescript w Vue#Reactive()]]
