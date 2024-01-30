---
layout: ../../layouts/BlogLayout.astro

title: "Vue - Podstawy"
pubDate: 2022-07-01
author: "Paweł Gałkowski"
image:
  url: "https://docs.astro.build/assets/full-logo-light.png"
  alt: "The full Astro logo."
tags: ["js", "ts", "frontend", "vue", "basics"]
---

Vue to JavaScriptowy framework bazowany na komponentach, przeznaczony do tworzenia interfejsów użytkownika.

```js
import { createApp, ref } from "vue";

createApp({
  setup() {
    return {s
      count: ref(0),
    };
  },
}).mount("#app");
```

```jsx
<div id="app">
  <button @click="count++">
    Count is: {{ count }}
  </button>
</div>
```

Przykład wyżej przedstawia dwa najważniejsze atrybuty vue:

- Deklaratywne renderowanie - rozszerza zwykły HTML poprzez możliwość dodania do niego danych ze stanu komponentu w JavaScript.
- Reaktywność - automatycznie śledzi zmiany w stanie JS i wydajnie update'uje DOM kiedy następują zmiany.

### Dwa style budowania komponentów

Komponenty Vue mogą być budowane używając dwóch różnych "styli" API

#### Options API

Standardowy styl znany z wcześniejszych wersji Vue. Skrypt w komponencie posiada objekt w `export default` w którym opisane jest zachowanie komponentu. Klucze tego objektu to m.in. `data`, `props`, `functions` czy też różne lifecycle komponentu. W tym obiekcie można odwołać się do samego obiektu poprzez zmienną `this`

```js

<script>
export default {
  // Properties returned from data() become reactive state
  // and will be exposed on `this`.
  data() {
    return {
      count: 0
    }
  },

  // Methods are functions that mutate state and trigger updates.
  // They can be bound as event handlers in templates.
  methods: {
    increment() {
      this.count++
    }
  },

  // Lifecycle hooks are called at different stages
  // of a component's lifecycle.
  // This function will be called when the component is mounted.
  mounted() {
    console.log(`The initial count is ${this.count}.`)
  }
}
</script>
```

```jsx
<template>
  <button @click="increment">Count is: {{ count }}</button>
</template>
```

#### Composition API

W tym stylu API używamy importowanych funkcji aby opisać zachowanie komponentu. Komponent nie ma wtedy exportu obiektu lecz używa importowanych funkcji. Najczęściej używa się go razem `<script setup>` dzięki czemu kod użyty w `setup` jest uruchamiany tylko raz, przy kompilacji. Zmienne zadeklarowane w setup są automatycznie dostępne w komponencie.

```js
<script setup>
  import {(ref, onMounted)} from 'vue' // reactive state const count = ref(0) //
  functions that mutate state and trigger updates function increment(){" "}
  {count.value++}
  // lifecycle hooks onMounted(() => {console.log(
    `The initial count is ${count.value}.`
  )})
</script>
```

```jsx
<template>
  <button @click="increment">Count is: {{ count }}</button>
</template>
```

### Różnice pomiędzy Vue 2 a Vue 3

Oprócz poprawy szybkości wykonywania nowa wersja Vue różni się także:

- Dodanie nowych elementów jak [Teleport] i [Suspense].
- Możliwość wielu elementów w root komponentu.
- Brak wsparcia IE11
- Reactivity bazowane na `proxies`, nie na `Object.defineProperty()`

#### Teleport

Nowy tag `<Teleport>` pozwala na wyjęcie zawartości komponentu z jego miejsca w DOM i umieszczenie go w innym. Nie zmienia to jego miejsca w logicznym drzewie Vue wiec nadal może korzystać z danych i metod komponentu w którym się znajduje, przenoszony jest tylko jego HTML.

```jsx
<Teleport to="body" :disabled="isDisabled">
	<div class="modal">
	//
	</div>
</Teleport>
```

#### Suspense

Komponent `<Suspense />` służy do organizacji asynchronicznych komponentów które w nim się znajdują. Dzięki temu komponentowi można łączyć wiele różnych komponentów asynchronicznych i takich z asynchronicznym `setup` w jeden Loading State.

```jsx
<Suspense>
  <!-- component with nested async dependencies -->
  <Dashboard />

  <!-- loading state via #fallback slot -->
  <template #fallback>
    Loading...
  </template>
</Suspense>
```

Komponent `<Suspense />` posiada dwa `sloty`: `#default` oraz `#fallback`. Oba mogą posiadać tylko po jednym dziecku. Jeżeli jest to możliwe wyświetlany jest `#default` jeżeli nie `#fallback`.

Podczas renderowania komponentu `<Suspense />` najpierw wyświetlany jest komponent z `#default` aż do momentu natrafienia na komponent asynchroniczny. Jeżeli taki się znajdzie `Suspense` przechodzi w stan `pending` i wyświetlany jest komponent z `#fallback` kiedy rozwiążą się wszystkie komponenty asynchroniczne `Suspense` przechodzi w stan `resolved`.

`Suspense` przejdzie spowrotem w tryb pending tylko wtedy kiedy zmieni się jego komponent-dziecko. Jeżeli zmiana nastąpi w dalszej części drzewa `Suspense` nie wejdzie w tryb `pending`.

Kiedy taka zmiana jednak nastąpi, nawet po przejściu w tryb `pending` nie będzie wyświetlany content z `#fallback` zamiast tego asynchroniczne komponenty będą się rozwiązywać podczas gdy widoczny jest poprzedni widok `#default`, chyba że taka zmiana będzie trwać dłużej niż podany czas w propie `timeout`, po przekroczeniu czasu pokaże się `#fallback`.

#### Zmiany w Reactivity [[Reactivity]]

W Vue 2 reactivity było bazowane na `Object.defineProperty()` który pozwalał na ustawienie danej zmiennej funkcji `get` i `set`. Minusem takiego systemu były problemy z reaktywnością objektów i ciągów danych, i przymus używania `this.$set` na nowych kluczach aby stworzyć je reaktywne.

Dzięki użyciu `Proxy` i `Reflect` w Vue 3 znika całkowicie problem z tworzeniem nowych wartosci dla reaktywnych obiektów.

### Lifecycle komponentów Vue

- setup - w trakcie kompilacji
- beforeCreated - dostępne są `props` ale nie `data` czy `computed`
- created - po tym kroku dostępna jest reaktywność, stan, metody oraz watchery
- beforeMounted - uruchamiany zaraz przed zamontowaniem komponentu. Stan komponentu jest dostępny lecz jeszcze nie ma DOM node. Nie jest uruchamiany przy SSR.
- mounted - komponent jest mounted jeżeli:
  - stworzone zostały DOM node jego, oraz jego rodzica
  - jeżeli zamontowane zostały jego synchroniczne dzieci.
- beforeUpdate - uruchamiany przed update DOM tego komponentu
- updated - uruchamiany po update DOM. Zmiany stanu w tym komponencie prowadzą do stworzenia infinite loop.
- beforeUnmount - uruchamiany przed odpięciem komponentu. Komponent jest wtedy nadal funkcjonalny, dostępne są stan, metody i watchery. Najczęściej używany do "posprzątania" po komponencie.
- unmounted - komponent jest uznany kiedy:

  - wszystkie jego dzieci są unmounted
  - wszystkie jego efekty (watchery, computed) zostały zatrzymane

- errorCaptured - uruchamiany kiedy został wychwycony błąd płynący z podległych komponentów
- activated - uruchamiany kiedy komponent zostanie włączony do DOM jako część drzewa komponentu `<KeepAlive />`
- deactivated - uruchamiany kiedy komponent zostaje wyłączony z drzewa komponentu `<KeepAlive />`
- serverPrefetch - **SSR Only** asyncroniczna funkcja która musi być być rozwiązana przed renderowaniem komponentu na serwerze. Powinno zwracać promise.

### Composables

To nowy sposób na unikanie duplikacji kodu w Vue, dzięki nim można tworzyć reusable funkcje które można używać w wielu komponentach.

```js
import { ref } from "vue";

export default function watchScroll() {
  let x = ref(0);
  window.addEventListener("scroll", function () {
    x.value = window.scrollY;
    console.log(x.value);
  });
  return x;
}
```

```js
<script setup>

import watchScroll from './composable.js'

let x = watchScroll()

</script>

<template>
  <div style="height: 500vh; position:relative;">
    <div style="position:fixed; top: 0">
      {{x}}
    </div>
  </div>
</template>
```

Composables mogą dostawać argumenty, zwracać dane asynchroniczne.

Argumentami Composables mogą być także dane reaktywne, dzięki czemu przy każdej ich zmianie funkcja Composable uruchamia się znowu, dzięki czemu można robić reusable kod którego wyniki będą się różnić w zależności od stanu komponentu, np. composable może być funkcja fetchująca dane z API i przyjmująca jako argument ścieżke z której dane mają być pobrane.

#### Różnice między Composables i Mixin's

Oba służą do podobnych rzeczy i celem ich zastosowania jest uproszczenie i brak duplikacji kodu w komponentach. Ale Composables są lepszym systemem niż Mixin'y ze względu na to że są one zamknięte w sobie, akceptują dane i zwracają rezultaty, mogą też robić side effecty (np. dodawanie czegoś do DOM itp.) ale w odróżnieniu od mixinów nie dodają do komponentu swoich wartości, danych i metod. Dzięki czemu unika się kolizji nazw i bałaganu przy użyciu większej ilości mixinów, które czasem zamiast uporządkowywać kod same sprawiały więcej problemów z utrzymaniem kodu.
