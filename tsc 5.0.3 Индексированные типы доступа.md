## Индексированные типы доступа

Мы можем использовать индексированный тип доступа для поиска определенного свойства в другом типе:

```ts
type Person = { age: number; name: string; alive: boolean };
type Age = Person["age"]; // type Age = number
```

Тип индексирования сам по себе является типом, поэтому мы можем полностью использовать объединения, `keyof` или другие типы:

```ts
type I1 = Person["age" | "name"]; // type I1 = string | number

type I2 = Person[keyof Person]; // type I2 = string | number | boolean

type AliveOrName = "alive" | "name";
type I3 = Person[AliveOrName]; // type I3 = string | boolean
```

Вы даже увидите ошибку, если попытаетесь проиндексировать несуществующее свойство:

```ts
type I1 = Person["alve"]; // Свойство 'alve' не существует в типе 'Person'.
```

Другим примером индексирования с произвольным типом является использование числа для получения типа элементов массива. Мы можем объединить это с типом, чтобы удобно захватить строковой тип элемента массива:

```ts
const MyArray = [
	{ name: "Alice", age: 15 },
	{ name: "Bob", age: 23 },
	{ name: "Eve", age: 38 },
];

type Person = typeof MyArray[number]; // type Person = {
									  //   name: string;
								      //   age: number;
									  // }

type Age = typeof MyArray[number]["age"]; // type Age = number

// Or
type Age2 = Person["age"]; // type Age2 = number
```

Типы можно использовать только при индексировании, т.е. нельзя использовать const для создания ссылки на переменную:

```ts
const key = "age";
type Age = Person[key];
// Тип 'key' не может использоваться в качестве типа индекса.
// 'key' ссылается на значение, но используется в качестве типа здесь. Вы имели в виду 'typeof key'?
```

Однако для аналогичного стиля рефактора можно использовать псевдоним типа:

```ts
type key = "age";
type Age = Person[key];
```