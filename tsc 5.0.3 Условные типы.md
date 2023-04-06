## Условные типы

В основе большинства полезных программ - принятие решений на основе вводимых данных. Программы JavaScript ничем не отличаются, но с учетом того, что значения могут быть легко интроспектированы, эти решения также основаны на типах входных данных. Условные типы помогают описать связь между типами входов и выходов.

```ts
interface Animal {
	live(): void;
}

interface Dog extends Animal {
	woof(): void;
}

type Example1 = Dog extends Animal ? number : string; // type Example1 = number

type Example2 = RegExp extends Animal ? number : string; // type Example2 = string
```

Условные типы принимают форму, которая немного похожа на условные выражения (условие ? выражение истины : выражение ложь) в JavaScript:

```ts
SomeType extends OtherType ? TrueType : FalseType;
```

Если тип слева от extends можно назначить типу справа, то вы получите тип в первой ветви (“true” ветвь); в противном случае вы получите тип в последней ветви (ветвь «false»).

На примерах выше условные типы могут не сразу показаться полезными - мы можем сказать себе, расширяет или нет Dog Animal и выбрать число или строку! Но сила условных типов исходит от использования их с дженериками.

Например, возьмем следующую функцию `createLabel`:

```ts
interface IdLabel {
	id: number /* некоторые поля */;
}

interface NameLabel {
	name: string /* другие поля */;
}

function createLabel(id: number): IdLabel;
function createLabel(name: string): NameLabel;
function createLabel(nameOrId: string | number): IdLabel | NameLabel;
function createLabel(nameOrId: string | number): IdLabel | NameLabel {
	throw "unimplemented";
}
```

Эти перегрузки для `createLabel` описывают одну функцию JavaScript, которая делает выбор на основе типов ее входных данных. Обратите внимание на несколько моментов:

1. Если библиотека должна делать такой же выбор снова и снова на протяжении всего API, это становится громоздким.
2. Мы должны создать три перегрузки функции: одну для каждого случая, когда мы уверены в типе (одна для строки и одна для числа), и одну для самого общего случая (принимая `string | number`). Для каждого нового типа, который может обрабатывать `createLabel`, количество перегрузок увеличивается экспоненциально.

Вместо этого мы можем кодировать эту логику в условном типе:

```ts
type NameOrId<T extends number | string> = T extends number
	? IdLabel
	: NameLabel;
```

Затем мы можем использовать этот условный тип, чтобы упростить наши перегрузки вплоть до одной функции без перегрузок.

```ts
function createLabel<T extends number | string>(idOrName: T): NameOrId<T> {
	throw "unimplemented";
}

let a = createLabel("typescript"); // let a: NameLabel

let b = createLabel(2.8); // let b: IdLabel

let c = createLabel(Math.random() ? "hello" : 42); // let c: NameLabel | IdLabel
```

### Ограничения условного типа

Часто чеки условного типа предоставляют нам какую-то новую информацию. Точно так же, как при сужении с защитой типа может дать нам более конкретный тип, истинная ветвь условного типа еще больше ограничит дженерики тем типом, по которому мы проверяем.

Например, возьмем следующее:

```ts
type MessageOf<T> = T["message"]; // Тип '"message"' нельзя использовать для индексирования типа 'T'.
```

В этом примере ошибки TypeScript, так как `T` не имеет свойство, называемое сообщением. Мы можем ограничить `T`, и TypeScript больше не будет жаловаться:

```ts
type MessageOf<T extends { message: unknown }> = T["message"];

interface Email {
	message: string;
}

type EmailMessageContents = MessageOf<Email>; // type EmailMessageContents = string
```

Однако, что делать, если мы хотим, чтобы MessageOf принял какой-либо тип, и по умолчанию что-то вроде never, если свойство сообщения недоступно? Это можно сделать, переместив ограничение и введя условный тип:

```ts
type MessageOf<T> = T extends { message: unknown } ? T["message"] : never;

interface Email {
	message: string;
}

interface Dog {
	bark(): void;
}

type EmailMessageContents = MessageOf<Email>; // type EmailMessageContents = string

type DogMessageContents = MessageOf<Dog>; // type DogMessageContents = never
```

В пределах истинной ветви TypeScript знает, что `T` будет иметь свойство `message`.

В качестве другого примера можно также записать тип `Flatten`, который выравнивает типы массивов к их типам элементов, и ни чего не делает в противном случае:

```ts
type Flatten<T> = T extends any[] ? T[number] : T;

// Извлечение типа элемента.
type Str = Flatten<string[]>; // type Str = string

// Оставляет тот же тип.
type Num = Flatten<number>; // type Num = number
```

Если для параметра `Flatten` задан тип массива, для извлечения типа элемента `string[]` используется индексированный доступ по индексу. В противном случае он просто возвращает тип, который был задан.

### Выведение в условных типах

Мы просто нашли с помощью условных типов, чтобы применить ограничения, а затем извлечь типы. Это в конечном итоге такая обычная операция, что условные типы облегчают ее.

Условные типы предоставляют нам способ выведения из типов, с которыми мы сравниваем в истинной ветви, используя ключевое слово `infer`. Например, мы могли бы вывести тип элемента в `Flatten` вместо извлечения его «вручную» с индексированным типом доступа:

```ts
type Flatten<Type> = Type extends Array<infer Item> ? Item : Type;
```

Здесь мы использовали логическое ключевое слово, чтобы декларативно ввести новую переменную общего типа с именем `Item` вместо указания того, как извлечь тип элемента `T` в истинной ветви. Это освобождает нас от необходимости думать о том, как копаться и исследовать структуру типов, которые нас интересуют.

Мы можем написать несколько полезных псевдонимов типа, используя ключевое слово `infer`. Например, для простых случаев можно извлечь возвращаемый тип из типов функций:

```ts
type GetReturnType<Type> = Type extends (...args: never[]) => infer Return
	? Return
	: never;
	
type Num = GetReturnType<() => number>; // type Num = number

type Str = GetReturnType<(x: string) => string>; // type Str = string

type Bools = GetReturnType<(a: boolean, b: boolean) => boolean[]>; // type Bools = boolean[]
```

При выводе из типа с несколькими сигнатурами вызова (типа перегруженной функции) выводы делаются из последней сигнатуры (что, предположительно, является наиболее правильным вариантом для всех). Невозможно выполнить разрешение перегрузки на основе списка типов аргументов.

```ts
declare function stringOrNum(x: string): number;
declare function stringOrNum(x: number): string;
declare function stringOrNum(x: string | number): string | number;

type T1 = ReturnType<typeof stringOrNum>; // type T1 = string | number
```

### Дистрибутивные условные типы

Когда условные типы действуют на универсальный тип, они становятся распределительными, если передается тип объединения. Например, выполните следующие действия.

```ts
type ToArray<Type> = Type extends any ? Type[] : never;
```

Если подключить тип объединения к `ToArray`, то условный тип будет применен к каждому члену этого объединения.

```ts
type ToArray<Type> = Type extends any ? Type[] : never;

type StrArrOrNumArr = ToArray<string | number>; // type StrArrOrNumArr = string[] | number[]
```

Что происходит здесь, так это то, что `StrArrOrNumArr` распространяет на:

```ts
 string | number;
```

и картирует по каждому типу членов объединения, что эффективно:

```ts
ToArray<string> | ToArray<number>;
```

что оставляет нас с:

```ts
string[] | number[];
```

Как правило, распределение является желаемым поведением. Чтобы избежать такого поведения, можно окружить каждую сторону ключевого слова extensions квадратными скобками.

```ts
type ToArrayNonDist<Type> = [Type] extends [any] ? Type[] : never;

// 'StrArrOrNumArr' больше не является объединением.
type StrArrOrNumArr = ToArrayNonDist<string | number>; // type StrArrOrNumArr = (string | number)[]
```