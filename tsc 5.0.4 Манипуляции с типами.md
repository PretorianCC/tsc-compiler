## Манипуляции с типами

Если вы не хотите повторяться, иногда тип должен быть основан на другом типе.

Сопоставленные типы основаны на синтаксисе сигнатур индекса, которые используются для объявления типов свойств, не объявленных ранее:

```ts
type OnlyBoolsAndHorses = {
	[key: string]: boolean | Horse;
};

const conforms: OnlyBoolsAndHorses = {
	del: true,
	rodney: false,
};
```

Сопоставленный тип является универсальным типом, который использует объединение `PropertyKeys` (часто созданное с помощью ключа) для итерации через ключи для создания типа:

```ts
type OptionsFlags<Type> = {
	[Property in keyof Type]: boolean;
};
```

В этом примере `OptionsFlags` возьмет все свойства типа `Type` и изменит их значения на логические.

```ts
type FeatureFlags = {
	darkMode: () => void;
	newUserProfile: () => void;
};

type FeatureOptions = OptionsFlags<FeatureFlags>; // type FeatureOptions = {
												  //     darkMode: boolean;
											      //     newUserProfile: boolean;
												  // }
```

## Модификаторы сопоставления

Есть два дополнительных модификатора, которые могут быть применены во время сопоставления: `readonly` и `?` которые влияют на мутабельность и опциональность соответственно.

Эти модификаторы можно удалить или добавить с помощью префикса - или +. Если префикс не добавляется, предполагается +.

```ts
// Удаляет атрибуты «readonly» из свойств типа
type CreateMutable<Type> = {
	-readonly [Property in keyof Type]: Type[Property];
};

type LockedAccount = {
	readonly id: string;
	readonly name: string;
};

type UnlockedAccount = CreateMutable<LockedAccount>; // type UnlockedAccount = {
												     //     id: string;
												     //     name: string;
													 // }
```

```ts
// Удаляет «optional» атрибуты из свойств типа
type Concrete<Type> = {
	[Property in keyof Type]-?: Type[Property];
};

type MaybeUser = {
	id: string;
	name?: string;
	age?: number;
};

type User = Concrete<MaybeUser>; // type User = {
							     //     id: string;
							     //     name: string;
							     //     age: number;
								 // }
```

## Переназначение ключей через as

С TypeScript 4.1 и выше можно повторно переназначать ключи в сопоставленных типах через команду as в сопоставленном типе:

```ts
type MappedTypeWithNewProperties<Type> = {
	[Properties in keyof Type as NewKeyType]: Type[Properties]
}
```

Для создания новых имен свойств из предыдущих можно использовать такие функции, как типы литералов шаблонов:

```ts
type Getters<Type> = {
	[Property in keyof Type as `get${Capitalize<string & Property>}`]: () => Type[Property]
};

interface Person {
	name: string;
	age: number;
	location: string;
}

type LazyPerson = Getters<Person>; // type LazyPerson = {
								   //     getName: () => string;
							       //     getAge: () => number;
							       //     getLocation: () => string;
								   // }
```

Можно отфильтровать ключи, создав их без использования условного типа:

```ts
// Удалить свойство 'kind'
type RemoveKindField<Type> = {
	[Property in keyof Type as Exclude<Property, "kind">]: Type[Property]
};

interface Circle {
	kind: "circle";
	radius: number;
}

type KindlessCircle = RemoveKindField<Circle>; // type KindlessCircle = {
											   //     radius: number;
											   // }
```

Можно сопоставить произвольные объединения, не только объединения `string | number | symbol`, но и объединения любого типа:

```ts
type EventConfig<Events extends { kind: string }> = {
	[E in Events as E["kind"]]: (event: E) => void;
}

type SquareEvent = { kind: "square", x: number, y: : number };
type CircleEvent = { kind: "circle", radius: number };

type Config = EventConfig<SquareEvent | CircleEvent> // type Config = {
												     //     square: (event: SquareEvent) => void;
												     //     circle: (event: CircleEvent) => void;
													 // }
```

## Дальнейшие исследования

Сопоставленные типы хорошо работают с другими функциями с этого раздела, например, здесь сопоставленный тип использует условный тип, который возвращает либо `true`, либо `false` в зависимости от того, имеет ли объект свойство `pii`, установленное как `true`:

```ts
type ExtractPII<Type> = {
	[Property in keyof Type]: Type[Property] extends { pii: true } ? true : false;
};

type DBFields = {
	id: { format: "incrementing" };
	name: { type: string; pii: true };
};

type ObjectsNeedingGDPRDeletion = ExtractPII<DBFields>; // type ObjectsNeedingGDPRDeletion = {
													    //     id: false;
													    //     name: true;
														// }
```