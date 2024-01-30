---
layout: ../../layouts/BlogLayout.astro

title: "SCSS"
pubDate: 2022-07-01
author: "Paweł Gałkowski"
image:
  url: "https://docs.astro.build/assets/full-logo-light.png"
  alt: "The full Astro logo."
tags: ["styles", "scss", "css", "basics", "frontend", "preprocessor"]
---

**SCSS** (Sassy Cascading Stylesheets) bardziej rozbudowany wariat CSS pozwalający na używanie nested CSS oraz używanie funkcji, mixinów itp. musi być kompilowany do CSS'a przez jakiś [[Preprocessory i bundlery]].

## Różnice pomiędzy SaSS a SCSS

```

SASS (Syntactically Awesome Style Sheets) is a pre-processor scripting language that will be compiled or interpreted into CSS. SassScript is itself a scripting language whereas SCSS is the main syntax for the SASS which builds on top of the existing CSS syntax. It makes use of semicolons and brackets like CSS (Cascaded Style Sheets).
SASS and SCSS can import each other. Sass actually makes CSS more powerful with math and variable support. 

```

- SCSS posiada strukture CSS i wykorzystuje `;` oraz `{ }` , SASS nie stosuje ich:

```SAS
$primary-color: green
$primary-bg: red

body
	color: $primary-color
	background: $primary-bg
```

```SCSS

$bgcolor: blue;
$textcolor: red;
$fontsize: 25px;

body {
	background-color: $bgcolor;
	color: $textcolor;
	font-size: $fontsize;
}
```

- SCSS nie ma "strict indentation", w przeciwienstwie do SASS

## SCSS - zmienne

### [Deklarowanie wartości](https://sass-lang.com/documentation/variables/)

Zmienne w SCSS są deklarowane przez: `$nazwa-zmiennej: wartość`, i nie muszą w przeciwieństwie do zmiennych CSS być deklarowane w klasie. Zmienne SCSS są także imperatywne czyli zmiana. ich wartości nie wpływa na ich wartość we wcześniejszym użyciu, w CSS są deklaratywne i ich zmiana w jakimkolwiek momencie zmienia wartości we wszystkich użyciach tej klasy.

Jeżeli zadeklarujemy zmienną z taką samą nazwą jaka już istnieje nadpisujemy starą wartość. Użycie flagi `!default` pozwala na stworzenie danej zmiennej o ile nie jest już zadeklarowana, przez co jest pewność że nie nadpiszemy wcześniej zadeklarowanej zmiennej.

[Wbudowane zmienne](https://sass-lang.com/documentation/modules/) nie mogą być nadpisywane.

#### Scope zmiennych

Zmienne w SCSS są globalne o ile nie są zadeklarowane w bloku stylu, wtedy ograniczone są do scope danego bloku.

```SCSS
$global-variable: global value;

.content {
  $local-variable: local value;
  global: $global-variable;
  local: $local-variable; // local value
}

.sidebar {
  global: $global-variable; // global value;
  $global-variable: zmieniony global value;
  // This would fail, because $local-variable isn't in scope:
  // local: $local-variable;

  .child {
      global: $global-variable; // zmieniony global value;
  }
}
```

Zadeklarowanie istniejącej zmiennej globalnej w bloku kodu nie nadpisuje tej globalnej zmiennej. W danym bloku zmienna będzie zmieniona, że użyta będzie flaga `!default` za której pomocą można zadeklarować zmienną np w mixinie.

## [@ Rules](https://sass-lang.com/documentation/at-rules/)

- **[@import](https://sass-lang.com/documentation/at-rules/import/)** - importuje plik z podanej ścieżki do pliku w którym jest użyty, dosłownie wkleja SCSS z tamtego pliku. Wszystkie zmienne znajdujące się w zaimportowanym pliku są dostępne globalnie. Importowane są także wszyskie @importy ładowanego pliku co może stwarzać problemy podmieniając zmienne itp.
- **[@use](https://sass-lang.com/documentation/at-rules/use/)** - ma podobne zastosowanie jak @import, ale. z większą kontrolą nad importowanym plikiem. Plik ładowany jest jako [[Moduł SCSS]] i będzie zaimportowany tylko raz nie zależnie od tego ile razy się go zimportuje, kod się nie powtarza. Przez zastosowanie @include można używać mixinów, funkcji i zmiennych. Aby użyć zmiennych z w ten sposób zaimportowanego modułu trzeba dodać do zmiennej **namespace**, dzięki czemu nie ma kolizji nazw przy importowaniu.
  ```scss
  @use "src/corners";
  //@use "src/corners" as c;
  //@use "src/corners" as \*;
  ```

.button {
@include corners.rounded;
padding: 5px + corners.$radius;
}

````
- **[@forward](https://sass-lang.com/documentation/at-rules/forward/)** -  "przekazuje" podany w url plik do kolejnych plików po użyciu @forward na dany plik. Czyli pipe'uje importy do następnych plików. W pliku z @forward zmienne nie są dostępne, trzeba do tego użyć @use na ten sam plik.
- **[@mixin i @include](https://sass-lang.com/documentation/at-rules/mixin/)** - mixiny pozwalają na tworzenie kawałków kodu które mogą być użyte wielokrotnie w różnych miejscach przy użyciu @include który "wkleja" kod z mixinu w dane miejsce.
```scss
@mixin reset-list {
  margin: 0;
  padding: 0;
  list-style: none;
}

@mixin horizontal-list {
  @include reset-list;

  li {
    display: inline-block;
    margin: {
      left: -2px;
      right: 2em;
    }
  }
}

nav ul {
  @include horizontal-list;
}
````

Mixiny zachowują się jak funkcje zwracającce kod css, mogą mieć też własne argumenty, podawane przez `@mixin nazwa($zmienna1, $zmienna2: defaultValue)` .
