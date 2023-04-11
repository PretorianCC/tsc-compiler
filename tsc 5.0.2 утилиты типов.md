## Утилиты типов

TypeScript предоставляет несколько утилит для упрощения преобразования общих типов. Эти утилиты доступны по всему миру.

## `Awaited<Type>`

Этот тип предназначен для моделирования таких операций, как ожидание в асинхронных функциях или метод `.then()` в промисах, он рекурсивно развертывает промисы.

```ts
type A = Awaited<Promise<string>>;
type A = string

type B = Awaited<Promise<Promise<number>>>;
type B = number

type C = Awaited<boolean | Promise<number>>;
type C = number | boolean
```

## `Partial<Type>`

Построение типа со всеми свойствами типа, добавляет членам объекта модификатор `?`. Эта утилита возвращает тип, который представляет все подмножества данного типа.

```ts
interface Todo {
    title: string;
    description: string;
}

function updateTodo(todo: Todo, fieldsToUpdate: Partial<Todo>) {
    return { ...todo, ...fieldsToUpdate };
}

const todo1 = {
    title: "organize desk",
    description: "clear clutter",
};

const todo2 = updateTodo(todo1, {
    description: "throw out trash",
});
```

```ts
// Реализация
type Partial<T> = {
    [P in keyof T]?: T[P];
};
```

## `Required<Type>`

Создает тип, состоящий из всех свойств типа, удаляет все необязательные модификаторы `?`. Противоположность `Partial`.

```ts
interface Props {
    a?: number;
    b?: string;
}

const obj: Props = { a: 5 }; 

const obj2: Required<Props> = { a: 5 };
// Свойство 'b' отсутствует в типе '{a: number;}' , но обязательно в типе 'Required<Props>'.
```

```ts
// Реализация
type Required<T> = {
    [P in keyof T]-?: T[P];
};
```

## `Readonly<Type>`

Создает тип со всеми свойствами типа, у которых задано значение `readonly`, что означает, что свойства построенного типа не могут быть переназначены.

```ts
interface Todo {
    title: string;
}

const todo: Readonly<Todo> = {
    title: "Delete inactive users",
};

todo.title = "Hello";
// Невозможно переназначить 'title', так как это свойство доступно только для чтения.
```

```ts
// Реализация
type Readonly<T> = {
    readonly [P in keyof T]: T[P];
};
```

Эта утилита полезна для представления выражений которые завершаются сбоем во время присвоения (т.е. при попытке переназначения свойств замороженного объекта).

`Object.freeze`

```ts
function freeze<Type>(obj: Type): Readonly<Type>;
```

## `Record<Keys, Type>`

Создает тип объекта, ключи свойств которого имеют значение `Keys`, а значения свойств `Type`. Эту утилиту можно использовать для сопоставления свойств типа с другим типом. Данный тип определяет два параметра типа. В качестве первого параметра ожидается множество ключей представленных множеством `string` или `Literal String`. В качестве второго параметра ожидается конкретный тип данных, который будет ассоциирован с каждым ключом.

```ts
interface CatInfo {
    age: number;
    breed: string;
}

type CatName = "miffy" | "boris" | "mordred";

const cats: Record<CatName, CatInfo> = {
    miffy: { age: 10, breed: "Persian" },
    boris: { age: 5, breed: "Maine Coon" },
    mordred: { age: 16, breed: "British Shorthair" },
};

cats.boris; // const cats: Record<CatName, CatInfo>
```

```ts
// Реализация
type Record<K extends keyof any, T> = {
    [P in K]: T;
};
```

## `Pick<Type, Keys>`

Создает тип объекта, ключами свойств которого являются `Keys`, а значения свойств — `Type`. Эту утилиту можно использовать для сопоставления свойств типа с другим типом.

```ts
interface Todo {
    title: string;
    description: string;
    completed: boolean;
}

type TodoPreview = Pick<Todo, "title" | "completed">;

const todo: TodoPreview = {
    title: "Clean room",
    completed: false,
};

todo; // const todo: TodoPreview
```

```TypeScript
// Реализация
type Pick<T, K extends keyof T> = {
    [P in K]: T[P];
};
```

## `Omit<Type, Keys>`

Создает тип, выбирая все свойства из `Type`, а затем удаляя `Keys` (строковый литерал или объединение строковых литералов).

```ts
interface Todo {
    title: string;
    description: string;
    completed: boolean;
    createdAt: number;
}

type TodoPreview = Omit<Todo, "description">;

const todo: TodoPreview = {
    title: "Clean room",
    completed: false,
    createdAt: 1615544252770,
};

todo;

const todo: TodoPreview type TodoInfo = Omit<Todo, "completed" | "createdAt">;

const todoInfo: TodoInfo = {
    title: "Pick up kids",
    description: "Kindergarten closes at 5pm",
};

todoInfo; // const todoInfo: TodoInfo
```

## `Exclude<UnionType, ExcludedMembers>`

Строит тип исключением из `UnionType` всех членов объединения, которые содержатся в `ExcludedMembers`. Обратное `Extract`.

```ts
type T0 = Exclude<"a" | "b" | "c", "a">; // type T0 = "b" | "c"

type T1 = Exclude<"a" | "b" | "c", "a" | "b">; // type T1 = "c"

type T2 = Exclude<string | number | (() => void), Function>; // type T2 = string | number
```

```ts
// Реализация
type Exclude<T, U> = T extends U ? never : T;
```

## `Extract<Type, Union>`

Создает тип путем извлечения из команды `Type`  всех членов объединения, которые содержатся в `Union`.

```ts
type T0 = Extract<"a" | "b" | "c", "a" | "f">; // type T0 = "a"

type T1 = Extract<string | number | (() => void), Function>; // type T1 = () => void
```

```ts
// Реализация
type Extract<T, U> = T extends U ? T : never;
```

## `NonNullable<Type>`

Создает тип путем исключения `null` и `undefined` из `Type`.

```ts
type T0 = NonNullable<string | number | undefined>; // type T0 = string | number

type T1 = NonNullable<string[] | null | undefined>; // type T1 = string[]
```

## `Parameters<Type>`

Создает тип кортежа из типов, используемых в параметрах типа функции `Type`.

```ts
declare function f1(arg: { a: number; b: string }): void;

type T0 = Parameters<() => string>; // type T0 = []type

T1 = Parameters<(s: string) => void>; // type T1 = [s: string]

type T2 = Parameters<<T>(arg: T) => T>; // type T2 = [arg: unknown]

type T3 = Parameters<typeof f1>; // type T3 = [arg: {
                                 //    a: number;
                                 //    b: string;
                                 // }]

type T4 = Parameters<any>; // type T4 = unknown[]
                     
type T5 = Parameters<never>; // type T5 = never
                     
type T6 = Parameters<string>; // Тип 'string' не удовлетворяет ограничению '(...args: any) => any'. type T6 = never
                     
type T7 = Parameters<Function>; // Тип 'Function' не удовлетворяет ограничению '(...args: any) => any'.
                                // Тип 'Function' не соответствует сигнатуре '(...args: any): any'.
                                // type T7 = never
```

```ts
// Реализация
type Parameters<T extends (...args: any) => any> = T extends (...args: infer P) => any ? P : never;
```

## `ConstructorParameters<Type>`

Создает тип кортежа или массива из типов функции конструктора. Он создает тип кортежа со всеми типами параметров (или ничего, если тип не является функцией).

```ts
type T0 = ConstructorParameters<ErrorConstructor>; // type T0 = [message?: string]

type T1 = ConstructorParameters<FunctionConstructor>; // type T1 = string[]

type T2 = ConstructorParameters<RegExpConstructor>; // type T2 = [pattern: string | RegExp, flags?: string]

type T3 = ConstructorParameters<any>; // type T3 = unknown[]

type T4 = ConstructorParameters<Function>; // Тип 'Function' не удовлетворяет ограничению 'abstract new (...args: any) => any'.
                                           // Тип 'Function' не соответствует сигнатуре 'new (...args: any): any'.
                                           // type T4 = never
```

```ts
// Реализация
type ConstructorParameters<T extends abstract new (...args: any) => any> = T extends abstract new (...args: infer P) => any ? P : never;
```

## `ReturnType<Type>`

Создает тип, состоящий из возвращаемого типа функции `Type`.

```ts
declare function f1(): { a: number; b: string };

type T0 = ReturnType<() => string>; // type T0 = string

type T1 = ReturnType<(s: string) => void>; // type T1 = void

type T2 = ReturnType<<T>() => T>; // type T2 = unknown

type T3 = ReturnType<<T extends U, U extends number[]>() => T>; // type T3 = number[]

type T4 = ReturnType<typeof f1>; // type T4 = {
                                 //     a: number;
                                 //     b: string;
                                 // }

type T5 = ReturnType<any>; // type T5 = any
                     
type T6 = ReturnType<never>; // type T6 = never
                     
type T7 = ReturnType<string>; // Тип 'string' не удовлетворяет ограничению '(...args: any) => any'.
                              // type T7 = any
     
type T8 = ReturnType<Function>; // Тип 'Function' не удовлетворяет ограничению '(...args: any) => any'.
                                // Тип 'Function' не соответствует сигнатуре '(...args: any): any'.
                                // type T8 = any
```

```TypeScript
// Реализация
type ReturnType<T extends (...args: any) => any> = T extends (...args: any) => infer R ? R : any;
```

## `InstanceType<Type>`

Создает тип, состоящий из типа экземпляра функции конструктора в `Type`.

```ts
class C {
    x = 0;
    y = 0;
}

type T0 = InstanceType<typeof C>; // type T0 = C
                       
type T1 = InstanceType<any>; // type T1 = any
                       
type T2 = InstanceType<never>; // type T2 = never
                       
type T3 = InstanceType<string>; // Тип 'string' не удовлетворяет ограничению 'abstract new (...args: any) => any'.
                                // type T3 = any
                       
type T4 = InstanceType<Function>; // Тип 'Function' не удовлетворяет ограничению 'abstract new (...args: any) => any'.
                                  // Тип 'Function' не соответствует сигнатуре 'new (...args: any): any'.
                                  // type T4 = any
```

```TypeScript
// Реализация
type InstanceType<T extends abstract new (...args: any) => any> = T extends abstract new (...args: any) => infer R ? R : any;
```

## `ThisParameterType<Type>`

Извлекает тип этого параметра для типа функции или unknown, если тип функции не имеет этого параметра.

```ts
function toHex(this: Number) {
    return this.toString(16);
}

function numberToString(n: ThisParameterType<typeof toHex>) {
    return toHex.apply(n);
}
```

## `OmitThisParameter<Type>`

Удаляет этот параметр из типа. Если `Type` не имеет явно объявленного этого параметра, результатом будет просто `Type`. В противном случае из типа создается новый тип функции без этого параметра. Дженерики удаляются, и в новый тип функции распространяется только последняя сигнатура перегрузки.

```ts
function toHex(this: Number) {
    return this.toString(16);
}

const fiveToHex: OmitThisParameter<typeof toHex> = toHex.bind(5);
console.log(fiveToHex());
```

## `ThisType<Type>`

Эта утилита не возвращает преобразованный тип. Вместо этого он служит маркером для контекста типа. Обратите внимание, что для использования этой утилиты должен быть включен флаг `noImplicitThis`.

```ts
type ObjectDescriptor<D, M> = {
    data?: D;
    methods?: M & ThisType<D & M>; // Тип 'this' в методах - D & M
};

function makeObject<D, M>(desc: ObjectDescriptor<D, M>): D & M {
    let data: object = desc.data || {};
    let methods: object = desc.methods || {};
    return { ...data, ...methods } as D & M;
}

let obj = makeObject({
    data: { x: 0, y: 0 },
    methods: {
        moveBy(dx: number, dy: number) {
            this.x += dx; // Сильно типизировалное this
            this.y += dy; // Сильно типизировалное this
        },
    },
});

obj.x = 10;
obj.y = 20;
obj.moveBy(5, 5);
```

В приведенном выше примере методы объекта в аргументе `makeObject` имеют контекстный тип, включающий `ThisType<D & M>`, и поэтому тип этого метода в методах объекта methods - `{x: number, y: number} & {mireBy (dx: number, dy: number): number}`. Обратите внимание, что тип свойства методов одновременно является выводом и источником для этого типа в методах.

Интерфейс `ThisType<T>` является просто пустым интерфейсом, объявленным в файле `lib.d.ts`. Помимо распознавания в контексте типа литерала объекта, интерфейс действует как любой пустой интерфейс.

## Типы управления строкой

### `Uppercase<StringType>`

### `Lowercase<StringType>`

### `Capitalize<StringType>`

### `Uncapitalize<StringType>`

Чтобы помочь со строковой манипуляцией вокруг шаблонных строковых литералов, TypeScript включает набор типов, которые могут использоваться в строковой манипуляции в системе типов. Их можно найти в документации по типам строковых шаблонов.

Tags: #TypeScript