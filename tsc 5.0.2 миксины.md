## Миксины

Наряду с традиционными иерархиями OO, другим популярным способом построения классов из многократно используемых компонентов является их построение путем объединения более простых частичных классов. Вы можете быть знакомы с идеей миксинов или трейтов для таких языков, как Scala, и шаблон также достиг некоторой популярности в сообществе JavaScript.

### Как работает миксин?

Шаблон основан на использовании дженериков с наследованием класса для расширения базового класса. Лучшая поддержка mixin в TypeScript выполняется с помощью шаблона выражения класса. Подробнее о том, как работает этот шаблон, можно узнать в JavaScript здесь.

Чтобы начать, нам нужен класс, который будет применять миксины поверх:

```ts
class Sprite {
	name = "";
	x = 0;
	y = 0;
	
	constructor(name: string) {
		this.name = name;
	}
}
```

Затем требуется тип и функция фабрики, которая возвращает выражение класса, расширяющее базовый класс.

```ts
// Для начала нам нужен тип, который мы будем использовать для расширения
// другие классы от. Основная обязанность - объявить
// что передаваемый тип является классом.

type Constructor = new (...args: any[]) => {};

// Эта смесь добавляет свойство, с getters и setters
// для изменения его с инкапсулированной private property:

function Scale<TBase extends Constructor>(Base: TBase) {
	return class Scaling extends Base {
		// Mixins не может объявлять private/protected свойства
	    // однако можно использовать ES2020 private поля
	    _scale = 1;
	    setScale(scale: number) {
			this._scale = scale;
		}
		get scale(): number {
			return this._scale;
		}
	};
}
```

При всех этих настройках можно создать класс, который представляет базовый класс с примененными примесями:

```ts
// Создайте новый класс из класса Sprite,
// с помощью приложения Mixin Scale:
const EightBitSprite = Scale(Sprite);

const flappySprite = new EightBitSprite("Bird");
flappySprite.setScale(0.8);
console.log(flappySprite.scale);
```

## Связанные миксины

В вышеуказанной форме миксин не имеет базовых знаний о классе, что может затруднить создание желаемого дизайна.

Чтобы смоделировать это, измените исходный тип конструктора, чтобы принять базовый аргумент.

```ts
// Это был наш предыдущий
constructor:type Constructor = new (...args: any[]) => {};
// Теперь мы используем универсальную версию, которая может применить ограничение к
// классу, к которому применяется эта примесь
type GConstructor<T = {}> = new (...args: any[]) => T;
```

Это позволяет создавать классы, которые работают только с ограниченными базовыми классами:

```ts
type Positionable = GConstructor<{ setPos: (x: number, y: number) => void }>;
type Spritable = GConstructor<Sprite>;
type Loggable = GConstructor<{ print: () => void }>;
```

Затем вы можете создать миксины, которые работают только тогда, когда у вас есть определенная база для построения:

```ts
function Jumpable<TBase extends Positionable>(Base: TBase) {
	return class Jumpable extends Base {
		jump() {
			// This mixin will only work if it is passed a base
			// class which has setPos defined because of the
			// Positionable constraint.
			this.setPos(0, 20);
	    }
	};
}
```

### Альтернативный шаблон

В предыдущих версиях этого документа рекомендовался способ записи миксинов, при котором иерархия среды выполнения и иерархии типов создаются отдельно, а затем объединяются в конце:

```ts
// Каждый миксин является традиционным классом ES
class Jumpable {
	jump() {}
}

class Duckable {
	duck() {}
}

// Включая базу
class Sprite {
	x = 0;
	y = 0;
}

// Затем создается интерфейс, объединяющий
// ожидаемые миксины с тем же именем, что и база.
interface Sprite extends Jumpable, Duckable {}
// Применение миксинов к базовому классу
// через JS во время выполнения
applyMixins(Sprite, [Jumpable, Duckable]); 

let player = new Sprite();
player.jump();
console.log(player.x, player.y);

// Это может жить в любом месте вашей кодовой базы:
function applyMixins(derivedCtor: any, constructors: any[]) {
	constructors.forEach((baseCtor) => {
		Object.getOwnPropertyNames(baseCtor.prototype).forEach((name) => {
			Object.defineProperty(
				derivedCtor.prototype,
				name,
				Object.getOwnPropertyDescriptor(baseCtor.prototype, name) ||
					Object.create(null)
			);
		});
	});
}
```

Этот шаблон меньше полагается на компилятор и больше на базу кода, чтобы обеспечить правильную синхронизацию времени выполнения и типа системы.

### Ограничения

Шаблон миксина поддерживается внутри компилятора TypeScript анализом потока кода. Есть несколько случаев, когда это не сработает.

##### Декораторы и миксины [`#4881`](https://github.com/microsoft/TypeScript/issues/4881)

Нельзя использовать декораторы для предоставления миксинов с помощью анализа потока кода:

```ts
// Функция декоратора, которая воспроизводит шаблон миксина:
const Pausable = (target: typeof Player) => {
	return class Pausable extends target {
		shouldFreeze = false;
	};
};

@Pausable
class Player {
	x = 0;  
	y = 0;
}

// В классе Player не объединен тип декоратора:
const player = new Player();
player.shouldFreeze; // Свойство shouldFreeze не существует в типе Player.

// Аспект среды выполнения может быть реплицирован вручную
// посредством компоновки типа или объединения интерфейса.
type FreezablePlayer = Player & { shouldFreeze: boolean };

const playerTwo = (new Player() as unknown) as FreezablePlayer;
playerTwo.shouldFreeze;
```

##### Статическое свойство миксинов [`#17829`](https://github.com/microsoft/TypeScript/issues/17829)

Скорее глюк, чем ограничение. Шаблон выражения класса создает синглтоны, поэтому они не могут быть сопоставлены в системе типов для поддержки различных типов переменных.

Вы можете обойти это, используя функции, чтобы вернуть классы, которые на основе дженериков:

```ts
function base<T>() {
	class Base {
		static prop: T;
	}
	return Base;
}

function derived<T>() {
	class Derived extends base<T>() {
		static anotherProp: T;
	}
	return Derived;
}

class Spec extends derived<string>() {}

Spec.prop; // строка
Spec.anotherProp; // строка
```