## Keyof Type оператор

Оператор `keyof` принимает тип объекта и создает строковое или числовое литеральное объединение его ключей. Следующий тип `P` имеет тот же тип, что и `«x» | «y»`:

```ts
type Point = { x: number; y: number };
type P = keyof Point; // type P = keyof Point
```

Если тип имеет сигнатуру строкового или числового индекса, `keyof` возвращает эти типы:

```ts
type Arrayish = { [n: number]: unknown };

type A = keyof Arrayish; // type A = number

type Mapish = { [k: string]: boolean };
type M = keyof Mapish; // type M = string | number
```

Обратите внимание, что в этом примере `M` - `string | number` - это потому, что ключи объектов JavaScript всегда принудительно преобразуются в строку, поэтому `obj[0]` всегда совпадает с `obj["0"]`.

keyof типы становятся особенно полезными в сочетании с отображением типов, о которых мы узнаем позже.