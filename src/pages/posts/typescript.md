---
layout: ../../layouts/BlogLayout.astro

title: "TypeScript w Vue"
pubDate: 2022-07-01
author: "Paweł Gałkowski"
image:
  url: "https://docs.astro.build/assets/full-logo-light.png"
  alt: "The full Astro logo."
tags: ["js", "ts", "frontend", "backend", "preprocessor"]
---

Typescript to język programowania budowany na JavaScriptcie, dając mu dodatkowe możliwości głównie typowanie do zmiennych. TypeScript jest językiem silnie typowanym w odróżnieniu od JS. Dzięki silnemu typowaniu:

- Lepsze feedbacki z errorami, IDE dzięki wiedzy na temat typu zmiennej sprawdza poprawność kodu jeszcze przed runtimem, dzięki czemu robimy mniej błędów i tworzymy kod sprawniej i szybciej.
- Lepsze autocomplete i podpowiedzi
- Tworzenie custom'owych typów dzięki interface'om, klasom i type aliasom.
- Kod staje się "samo dokumentujący się"

### Instalowanie i budowanie Typescript

```shell
npm install typescript
```

Samo zainstalowanie TypeScript'a nie pozwala go użyć, TS musi być kompilowany do JS'a przez komendę:

```shell
tsc nazwa-pliku.ts
```

Dzięki której plik .ts jest przebudowywane do pliku .js który możemy używać już normalnie, czy to przez node czy w przeglądarce.

Lepszym sposobem na budowanie plików .ts jest stworzenie pliku konfiguracyjnego `tsconfig.json` i w nim opisanie jak typescript ma się budować. Można go wygenerować przez komendę:

```shell
tsc --init
```

Dzięki `rootDir` i `outputDir` możemy ustawić typescriptowi jaki folder i pliki mają być budowane, i gdzie wyniki tego budowania mają trafiać.

Kiedy plik `tsconfig.json` istnieje, wystarczy wpisać komende:

```shell
tsc --watch
```

aby budować pliki typescript z ustawieniami z pliku `tsconfig.json`, a dzięki fladze `--watch` będzie to się działo przy każdej zmianie pliku w folderze ustalonym w. `rootDir`.

### Podstawy typów

W poniższy sposób można przypisywać typ do zmiennej:

```typescript
let age: number = 30;
let firstname: string = "name";
let isFictional: boolean = false;
```

Ale jeżeli przy tworzeniu zmiennej od razu przypisujemy do niej wartość, wtedy typowanie można całkowicie pominąć ze względu na to że typescript automatycznie przypisze do zmiennej typ przypisanej wartości.

Zmienne nie muszą mieć przypisanej jednej wartości, można im przypisać dowolną ilość wartości poprzez:

```typescript
let stringOrNumber: string | number = "string";

stringOrNumber = 30;
```

Tak zapisana zmienna nie będzie błędna, bo TypeScript spodziewa się możliwej zmiany wartości z String na Number.

#### Array i Object

##### Array

Array'e są typowane poprzez:

```typescript
let array: string[] = ["a", "b", "c"];
```

W ten sposób każdy element tego ciągu musi mieć typ `string`, w innym przypadku TS powie nam o błędzie w typowaniu tego elementu. Tak samo wykonanie metody tego ciągu z błędnym typem argumentu będzie przez TS wychwytywane, powiedzmy że do poprzedniego przykładu dodam taki kod:

```typescript
array.push(1);
```

Wtedy dostanę błąd typu bo TS wie że do ciągu mogą być przypisane tylko typy `string`.

###### Object

Typowanie obiektów można wykonać w taki sposób:

```typescript
let object: {
  name: string;
  email: string;
  age: number;
} = {
  name: "Jan",
  email: "Kowalski",
  age: 30,
};
```

Tak typowany obiekt będzie pilnował typów w danych ale także struktury obiektu, błędny będzie brak jednej z danych lub dodanie nowej bez zmiany struktury typu, a więc błędne będzie:

```typescript
let object: {
  name: string;
  email: string;
  age: number;
} = {
  name: "Jan",
  email: "Kowalski",
};
```

oraz

```typescript
let object: {
  name: string;
  email: string;
  age: number;
} = {
  name: "Jan",
  email: "Kowalski",
  age: 30,
  id: 1,
};
```

#### Funkcje

Funkcje przyjmują typy dla argumentów, oraz typ zwracanych danych:

```typescript
function addTwo(a: number, b: number): number {
  return a + b;
}
```

W arrow function wyglądać to będzie tak:

```typescript
const addTwo = (a: number, b: number): number => {
  return a + b;
};
```

#### `Any` type

Typ `any` to specjalny typ który mówi interpreterowi że dana zmienna może być jakiegokolwiek typu, taka zmienna działa wtedy dokładnie tak samo jakby była stworzona w JavaScript, a nie w TypeScripcie. Odpadają nam dla niej wszystkie "bonusy" używania TS, ale także wszystkie możliwe problemy z nim związane.

Typ `any` to hakowy sposób aby pominąć sprawdzanie typu dla danej zmiennej.

#### Tuples

Tuple to ciąg zmiennych o określonych typach. W odróżnieniu od zwykłego ciągu w tuplach kolejność danych w ciągu ma duże znaczenie.

```typescript
let person: [string, number, boolean] = ["Paweł", 26, true];
```

Tuple mogą być też nazwane (Named Tuples), aby lepiej zakomunikować co powinno kryć się pod danym miejscem w ciągu:

```typescript
let person: [name: string, age: number, isCool: boolean] = ["Paweł", 26, true];
```

#### Interfaces

Jest to sposób na określenie kształtu struktury danego obiektu, i stworzenie go jako możliwy do użycia nowy 'typ' zmiennej.

```typescript

interface Person = {
	name: string,
	age: number
}

const personOne: Person = {
	name: "Adam",
	age: 27
}

```

Przy tworzeniu interface'ów można wartość określic jako opcjonalną dodając do niej symbol "?"

```typescript
interface Person = {
	name: string,
	age: number,
	tag?: string[]
}
```

Dzięki czemu interpreter oczekuje że taka wartość może znaleźć się w tym obiekcie, ale nie musi.
