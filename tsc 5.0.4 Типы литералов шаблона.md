## Типы литералов шаблона

Типы литералов шаблонов создаются на строковых типах литералов и могут расширяться во многие строки через объединения.

Они имеют тот же синтаксис, что и строки литералов шаблонов в JavaScript, но используются в позициях типа. При использовании с конкретными типами литералов литерал шаблона создает новый строковый тип литерала путем объединения содержимого.

```ts
type World = "world";

type Greeting = `hello ${World}`; // type Greeting = "hello world"
```

Когда объединение используется в интерполированной позиции, тип представляет собой набор всех возможных строковых литералов, которые могут быть представлены каждым членом объединения:

```ts
type EmailLocaleIDs = "welcome_email" | "email_heading";
type FooterLocaleIDs = "footer_title" | "footer_sendoff";
	
type AllLocaleIDs = `${EmailLocaleIDs | FooterLocaleIDs}_id`; // type AllLocaleIDs = "welcome_email_id" | "email_heading_id" | "footer_title_id" | "footer_sendoff_id
```

Для каждой интерполированной позиции в литерале шаблона происходит перекрестное умножение соединений:

```ts
type AllLocaleIDs = `${EmailLocaleIDs | FooterLocaleIDs}_id`;
type Lang = "en" | "ja" | "pt";

type LocaleMessageIDs = `${Lang}_${AllLocaleIDs}`; // type LocaleMessageIDs = "en_welcome_email_id" | "en_email_heading_id" | "en_footer_title_id" | "en_footer_sendoff_id" | "ja_welcome_email_id" | "ja_email_heading_id" | "ja_footer_title_id" | "ja_footer_sendoff_id" | "pt_welcome_email_id" | "pt_email_heading_id" | "pt_footer_title_id" | "pt_footer_sendoff_id"
```

Мы обычно рекомендуем людям использовать раннюю генерацию для больших строковых объединений, но это полезно в редких случаях.

## Объединения строк в типах

Сила литералов шаблона возникает при определении новой строки на основе информации внутри типа.

Рассмотрим случай, когда функция (`makeTaxingObject`) добавляет новую функцию, вызываемую `on()`, к переданному объекту. В JavaScript его вызов может выглядеть как: `makeTaxingObject(baseObject)`. Мы можем представить, как выглядит базовый объект:

```ts
const passedObject = {
	firstName: "Saoirse",
	lastName: "Ronan",
	age: 26,
};
```

Функция `on`, которая будет добавлена к базовому объекту, ожидает два аргумента: eventName (строка) и `callBack` (функция).

Свойство `eventName` должно иметь вид `attribureInTheTaxingObject + Changed` таким образом, `firstNameChanged` является производным от атрибута `firstName` в базовом объекте.

Функция `callBack` при вызове:

- Должно быть передано значение типа, связанного с именем `attribiveInTheTaxingObject`, таким образом, поскольку `firstName` вводится в виде строки, обратный вызов события `firstNameChanged` ожидает, что ему будет передана строка во время вызова. Аналогичным образом события, связанные с `age`, должны вызываться с аргументом числа
- Должен иметь тип возврата `void` (для простоты демонстрации)

Таким образом, сигнатура функции `on()` может быть: `on(eventName: string, callBack: (newValue: any) = > void)`. Однако в предыдущем описании мы определили важные ограничения типа, которые мы хотели бы документировать в нашем коде. Типы литералов шаблона позволяют внести эти ограничения в наш код.

```ts
const person = makeWatchedObject({
	firstName: "Saoirse",
	lastName: "Ronan",
	age: 26,}
);

// makeWatchedObject добавил 'on' к анонимному объекту

person.on("firstNameChanged", (newValue) => {
	console.log(`firstName was changed to ${newValue}!`);
});
```

Обратите внимание, что `on` прослушивает событие `firstNameChanged`, а не только `firstName`. Наша нативная спецификация `on()` могла бы быть более надежной, если бы мы гарантировали, что набор допустимых имен событий был ограничен объединением имен атрибутов в наблюдаемом объекте с добавлением «Changed» в конце. В то время как мы довольны выполнением такого вычисления в JavaScript т.е. `Object.keys (passedObject) .map (x => '$ {x} Changed')`, литералы шаблона в системе типа обеспечивают аналогичный подход к обработке строк:

```ts
type PropEventSource<Type> = {
	on(eventName: `${string & keyof Type}Changed`, callback: (newValue: any) => void): void;
};

/// Создание «наблюдаемого объекта» методом «on»
/// позволяет отслеживать изменения свойств.
declare function makeWatchedObject<Type>(obj: Type): Type & PropEventSource<Type>;
```

С помощью этого мы можем построить что-то, что с ошибками, когда дано неправильное свойство:

```ts
const person = makeWatchedObject({
	firstName: "Saoirse",
	lastName: "Ronan",
	age: 26
});

person.on("firstNameChanged", () => {});

// Предотвращение простой ошибки пользователя (использование ключа вместо имени события)
person.on("firstName", () => {}); // Аргумент типа '"firstName"' не может быть назначен параметру типа '"firstNameChanged" | "lastNameChanged" | "ageChanged"'.

// Это устойчивый к опечатке
person.on("frstNameChanged", () => {}); // Аргумент типа '"frstNameChanged"' не может быть назначен параметру типа '"firstNameChanged" | "lastNameChanged" | "ageChanged"'.
```

### Вывод с литералами шаблонов

Обратите внимание, что мы не выиграли от всей информации, предоставленной в первоначальном переданном объекте. При изменении `firstName` (т.е. события `firstNameChanged`) следует ожидать, что обратный вызов получит аргумент типа `string`. Аналогично, обратный вызов для изменения возраста должен получать аргумент `number`. Мы наивно используем `any` для ввода аргумента `callBack`. Кроме того, типы литералов шаблона позволяют гарантировать, что тип данных атрибута будет тем же самым типом, что и первый аргумент обратного вызова этого атрибута.

Ключевое понимание, которое делает это возможным, это то, что мы можем использовать дженерик функцию, которая:

1. Литерал, используемый в первом аргументе, фиксируется как литерал
2. Этот тип литерала может быть проверен как находящийся в объединении допустимых атрибутов в базовой модели.
3. Тип валидного атрибута можно найти в дженерик структуре с помощью индексированного доступа
4. Эта информация ввода затем может быть применена для обеспечения того, чтобы аргумент функции обратного вызова был того же типа

```ts
type PropEventSource<Type> = {
	on<Key extends string & keyof Type>
		(eventName: `${Key}Changed`, callback: (newValue: Type[Key]) => void ): void;
};

declare function makeWatchedObject<Type>(obj: Type): Type & PropEventSource<Type>;

const person = makeWatchedObject({
	firstName: "Saoirse",
	lastName: "Ronan",
	age: 26
});

person.on("firstNameChanged", newName => { // (параметр) newName: string
	console.log(`new name is ${newName.toUpperCase()}`);
});

person.on("ageChanged", newAge => { // (параметр) newAge: number
	if (newAge < 0) {
		console.warn("warning! negative age");
    }
})
```

Здесь мы превратили в дженерик метод.

Когда пользователь вызывает со строкой `firstNameChanged`, TypeScript пытается определить правильный тип ключа. Для этого он сопоставит `Key` с содержимым до «Changed» и выведет строку `firstName`. Как только TypeScript обнаружит это, метод on может получить тип `firstName` для исходного объекта, который в данном случае является строковым. Аналогично, при вызове с параметром `ageChanged` TypeScript находит тип для свойства, который является числом.

Вывод можно комбинировать по-разному, часто для деконструкции строк, и реконструировать их по-разному.

### Типы управления встроенной строкой

Для упрощения обработки строк TypeScript включает набор типов, которые можно использовать в обработке строк. Эти типы встроены в компилятор для обеспечения производительности и не могут быть найдены в файлах `.d.ts`, включенных в TypeScript.

#### `Uppercase<StringType>`

Преобразует каждый символ в строке в верхний регистр.

##### пример

```ts
type Greeting = "Hello, world"
type ShoutyGreeting = Uppercase<Greeting> // type ShoutyGreeting = "HELLO, WORLD"

type ASCIICacheKey<Str extends string> = `ID-${Uppercase<Str>}`
type MainID = ASCIICacheKey<"my_app"> // type MainID = "ID-MY_APP"
```

#### `Lowercase<StringType>`

Преобразует каждый символ в строке в эквивалент нижнего регистра.

##### пример

```ts
type Greeting = "Hello, world"
type QuietGreeting = Lowercase<Greeting> // type QuietGreeting = "hello, world"

type ASCIICacheKey<Str extends string> = `id-${Lowercase<Str>}`
type MainID = ASCIICacheKey<"MY_APP"> // type MainID = "id-my_app"
```

#### `Capitalize<StringType>`

Преобразует первый символ в строке в эквивалент верхнего регистра.

##### пример

```ts
type LowercaseGreeting = "hello, world";
type Greeting = Capitalize<LowercaseGreeting>; // type Greeting = "Hello, world"
```

#### `Uncapitalize<StringType>`

Преобразует первый символ в строке в эквивалент нижнего регистра.

##### пример

```ts
type UppercaseGreeting = "HELLO WORLD";
type UncomfortableGreeting = Uncapitalize<UppercaseGreeting>;  // type UncomfortableGreeting = "hELLO WORLD"
```

### Технические сведения о внутренних типах обработки строк

Код, начиная с TypeScript 4.1, для этих внутренних функций использует строковые функции выполнения JavaScript непосредственно для манипуляции и не учитывает языковые стандарты.

```ts
function applyStringMapping(symbol: Symbol, str: string) {
	switch (intrinsicTypeKinds.get(symbol.escapedName as string)) {
		case IntrinsicTypeKind.Uppercase: return str.toUpperCase();
		case IntrinsicTypeKind.Lowercase: return str.toLowerCase();
		case IntrinsicTypeKind.Capitalize: return str.charAt(0).toUpperCase() + str.slice(1);
		case IntrinsicTypeKind.Uncapitalize: return str.charAt(0).toLowerCase() + str.slice(1);
	}
	return str;
}   
```