---
layout: ../../layouts/BlogLayout.astro

title: "Podstawy PHP"
pubDate: 2022-07-01
author: "Paweł Gałkowski"
image:
  url: "https://docs.astro.build/assets/full-logo-light.png"
  alt: "The full Astro logo."
tags: ["php", "backend", "basics"]
---

PHP, czyli Hypertext Preprocessor, to **skryptowy język programowania, który został specjalnie zaprojektowany do tworzenia stron internetowych i aplikacji internetowych**. Jest językiem interpretowanym, co oznacza, że kod PHP jest wykonywany na serwerze, a wynikowy HTML jest przesyłany do przeglądarki użytkownika.

### Variables

W PHP zmienne zaczynają się znakiem `$` a następnie jest ich nazwa, zmienne są słabo typowane. Nie ma w PHP potrzeby deklarowania nowej zmiennej, są tworzone przy pierwszym przypisaniu watrtości do danej nazwy.

```php
$x = 5;
$y = "John"
```

Dostępne są następujące typy danych:

- String
- Integer
- Float (floating point numbers - also called double)
- Boolean
- Array
- Object
- NULL
- Resource

### Foreach i array'e

Array deklarujemy tak jak normalną zmienną:

```php
$array = [
	'string',
	5,
	false
];

echo $array[0] // 'string'
```

można też robić associative array, czyli array'e których dane mają nazwy:

```php
$person = [
	'name' => 'Adam',
	'age' => 25,
];

echo $person['name'] // 'Adam'
```

Na arrayach możemy robić użyć for each w celu stworzenia np list, normalnie wygląda to mniej więcej tak:

```php
<?php foreach($array as $single) {
	echo "<li>$single</li>"
	}
?>
```

lub:

```php
<?php foreach($array as $single) : ?>
	<li><?= $single?></li>
<?php endforeach; ?>
```

### Functions

Funkcje są deklarowane przez keyword `function` po którym wpisuje się nazwe funkcji i w nawiasach jej argumenty:

#### Named Function

```php
function addSum(a, b) {
	return a + b;
};

echo addSum(2, 5); // 7
```

#### Lambda Function

```php
$addSum = function (a, b) {
	return a + b;
};

echo $addSum(2, 5); // 7
```

### Importowanie plików .php

Dwa sposoby importowania plików php `require` i `import` działają w praktyce na zasadzie "wklejenia" bloku kodu importowanego pliku w miejscu w którym zadeklarowaliśmy import.

Dzięki temu można łączyć pliki .php ale także tworzyć `partiale` czyli reusable kawałki z kodem html które mogą używać zmiennych i danych zadeklarowanych wyżej w drzewie zależności.

### Class

Klasy to blueprinty, opisują strukture obiektu który można stworzyć. Mogą posiadać swoje wartości oraz funkcje, czyli metody.

```php
class Car {
	public $color;
	public $model;

	public function honk() {
		echo '{$this->model} honk!';
	}
}
```

Klasy mogą posiadać publiczne, prywatne oraz protected metody i zmienne, publiczne mogą być wykorzystywane poza scopem klasy, prywatne i protected tylko w scopie danej klasy. Czyli aby użyć metody prywatnej lub protected klasy, mogą być one wykorzystana tylko przez inne metody tej klasy. Nie można ich wywołać spoza nich.

```php
class MyClass   {
	public $public = 'Public';
	protected $protected = 'Protected';
	private $private = 'Private';

	function printHello()   {
	   echo $this->public;
	   echo $this->protected;
	   echo $this->private;
	}
}

$obj = new MyClass();

echo $obj->public; // Works
echo $obj->protected; // Fatal Error
echo $obj->private; // Fatal Error

$obj->printHello(); // Shows Public, Protected and Private`
```

Różnica pomiędzy protected a private pojawia się w momencie gdy dziedziczymy klase. Metoda private nie może być w klasie-dziecku wywoływana, a protected może.

```php
class MyClass2 extends MyClass   {
	// We can redeclare the public and protected properties, but not private
	public $public = 'Public2';
	protected $protected = 'Protected2';

	function printHello()   {
		echo $this->public;
		echo $this->protected;
		echo $this->private;
	}
}

$obj2 = new MyClass2();

echo $obj2->public; // Works
echo $obj2->protected; // Fatal Error
echo $obj2->private; // Undefined

$obj2->printHello(); // Shows Public2, Protected2, Undefined   `
```
