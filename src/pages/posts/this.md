---
layout: ../../layouts/BlogLayout.astro

title: "This"
pubDate: 2022-07-01
author: "Paweł Gałkowski"
image:
  url: "https://docs.astro.build/assets/full-logo-light.png"
  alt: "The full Astro logo."
tags: ["js", "ts", "basics", "frontend", "backend"]
---

## Podstawy this'a

`This` jest ukrytym parametrem funkcji który zawsze jest obiektem. To czym jest zależy od tego w jaki sposób funkcja jest wywoływana i różni się też w zależności czy użyty jest `use strict`, jeżeli strict mode jest włączony `this` może być też `undefined`.

Zmienna przypisywana do `this` jest zwykle obiektem z którego **wywoływana jest funkcja**.

```js
const obj = {
  val: "123",
  func: function () {
    return this.val;
  },
};

obj.func(); // '123'

const obj2 = {
  val: "234",
};

obj2.func = obj.func;

obj2.func(); // '234'

const globalFunc = obj.func;

globalFunc(); // undefined
```

W drugim przykładzie funkcja **wywołana** jest z `obj2` i dlatego `this` jest tym objektem.

W przykładzie 3cim `this` jest **Obiektem globalnym** ponieważ funkcja jest wywoływana nie z obiektu i w scopie globalnym. Objektem globalnym w przeglądarce jest objekt `window` (w `strict mode` `this` miałoby w tym przypadku `undefined`).

### Arrow Function

Jest różnica miedzy `function() {}` a `() => {}` i jest to właśnie różnica w przypisywaniu obiektu do `this`.

**Arrow functions nie posiadają "własnego" `this`'a** , dziedziczą go ze scope z którego są uruchamiane. Więc jeżeli w

### .bind()

Funkcja `Function.prototype.bind(arg)` daje nam możliwość przypisania argumenty jako this do danej funkcji. Dzięki czemu `this` nie zmieni się nie zależnie w jaki sposób dana funkcja zostaje wywołana.
