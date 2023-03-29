## Version 5.0.2
### ОБЩИЕ КОМАНДЫ

`tsc` - Компилирует текущий проект (tsconfig.json в рабочей папке).

`tsc app.ts util.ts` - Игнорируя файл tsconfig.json, компилирует указанные файлы с параметрами компилятора по умолчанию.

`tsc -b` - Создаёт составной проект в рабочей папке.

`tsc --init` - Создает файл tsconfig.json с рекомендуемыми параметрами в рабочей папке.

`tsc -p ./path/to/tsconfig.json` - Компилирует проект TypeScript, расположенный по указанному пути.

`tsc --help --all` - Расширенная версия этой информации, показывающая все возможные параметры компилятора.

`tsc --noEmit`, `tsc --target esnext` - Компилирует текущий проект с дополнительными параметрами.

### ФЛАГИ КОМАНДНОЙ СТРОКИ

`--help`, `-h` - Выводит на консоль справку.

`--watch`, `-w` - Наблюдатель отслеживающий измененные входные файлы.

`--all` - Показать все параметры компилятора.

`--version`, `-v` - Выводит на консоль версию компилятора.

`--init` - Инициализирует проект TypeScript и создает файл tsconfig.json.

`--project`, `-p` - Скомпилировать проект по пути к его файлу конфигурации или в папке с файлом tsconfig.json.

`--build`, `-b` - Создание одного или нескольких проектов и их зависимостей, если они устарели. Компиляция проекта.

`--showConfig` - Печать окончательной конфигурации вместо компиляции.

### ОБЩИЕ ПАРАМЕТРЫ КОМПИЛЯТОРА

`--pretty` - Включить цвет и форматирование в выходных данных TypeScript, чтобы упростить чтение ошибок компилятора.
Тип: boolean
По умолчанию: true

`--declaration`, `-d` - Создание файлов .d.ts из файлов TypeScript и JavaScript в проекте.
Тип: boolean
По умолчанию: `false`, если `composite` установлено

`--declarationMap` - Создание исходных схем для файлов d.ts.
Тип: boolean
По умолчанию: false

`--emitDeclarationOnly` - Выводить только файлы d.ts, а не JavaScript.
Тип: boolean
По умолчанию: false

`--sourceMap` - Создайть файлы карт источников для скомпилированных файлов JavaScript.
Тип: boolean
По умолчанию: false

`--target`, `-t` - Установить версию языка JavaScript для скомпилированного JavaScript и включить совместимые объявления библиотеки.
одно из: es3, es5, es6/es2015, es2016, es2017, es2018, es2019, es2020, es2021, es2022, esnext
По умолчанию: es5

`--module`, `-m` - Укажите, какой код модуля создается.
одно из: none, commonjs, amd, umd, system, es6/es2015, es2020, es2022, esnext, node16, nodenext
По умолчанию: undefined

`--lib` - Укажите набор пакетных файлов объявлений библиотек, описывающих целевую среду выполнения.
одно или несколько: es5, es6/es2015, es7/es2016, es2017, es2018, es2019, es2020, es2021, es2022, es2023, esnext, dom, dom.iterable, webworker, webworker.importscripts, webworker.iterable, scripthost, es2015.core, es2015.collection, es2015.generator, es2015.iterable, es2015.promise, es2015.proxy, es2015.reflect, es2015.symbol, es2015.symbol.wellknown, es2016.array.include, es2017.object, es2017.sharedmemory, es2017.string, es2017.intl, es2017.typedarrays, es2018.asyncgenerator, es2018.asynciterable/esnext.asynciterable, es2018.intl, es2018.promise, es2018.regexp, es2019.array, es2019.object, es2019.string, es2019.symbol/esnext.symbol, es2019.intl, es2020.bigint/esnext.bigint, es2020.date, es2020.promise, es2020.sharedmemory, es2020.string, es2020.symbol.wellknown, es2020.intl, es2020.number, es2021.promise/esnext.promise, es2021.string, es2021.weakref/esnext.weakref, es2021.intl, es2022.array, es2022.error, es2022.intl, es2022.object, es2022.sharedmemory, es2022.string/esnext.string, es2022.regexp, es2023.array/esnext.array, esnext.intl, decorators, decorators.legacy
По умолчанию: undefined

`--allowJs` - Разрешить участие файлов JavaScript в программе. Чтобы получить ошибки из этих файлов, воспользуйтесь опцией «startJS».
Тип: boolean
По умолчанию: false

`--checkJs` - Включить отчеты об ошибках в файлах JavaScript с проверкой типа.
Тип: boolean
По умолчанию: false

`--jsx` - Укажите, какой код JSX создается.
одно из: preserve, react, react-native, react-jsx, react-jsxdev
По умолчанию: undefined

`--outFile` - Укажите файл, объединяющий все выходные данные в один файл JavaScript. Если «declaration» имеет значение true, также обозначает файл, объединяющий все выходные данные .d.ts.

`--outDir` - Укажите папку вывода для всех скомпилированных файлов.

`--removeComments` - Отключить компиляцию комментариев.
Тип: boolean
По умолчанию: false

`--noEmit` - Отключить передачу файлов из компиляции.
Тип: boolean
По умолчанию: false

`--strict` - Включить все параметры строгой проверки типа.
Тип: boolean
По умолчанию: false

`--types` - Укажите имена пакетов типов для включения без ссылок в исходный файл.

`--esModuleInterop` - Используйте дополнительный JavaScript для упрощения поддержки импорта модулей CommonJS. При этом для совместимости типов включен параметр allowExingDefureImports.
Тип: boolean
По умолчанию: false

## ВСЕ ПАРАМЕТРЫ КОМПИЛЯТОРА

### Параметры командной строки

`--all` - Показать все параметры компилятора.

`--build`, `-b` - Создание одного или нескольких проектов и их зависимостей, если они устарели. Компиляция проекта.

`--help`, `-h`, `-?` - Выводит на консоль справку.

`--init` - Инициализирует проект TypeScript и создает файл tsconfig.json.

`--listFilesOnly` - Расшифровка подписи файлов, которые являются частью компиляции и затем прекращают обрабатываться.\

`--locale` - Задайте язык сообщений из TypeScript. Это не влияет на компиляцию.

`--project`, `-p` - Скомпилировать проект по пути к его файлу конфигурации или в папке с файлом tsconfig.json.

`--showConfig` - Печать окончательной конфигурации вместо компиляции.

`--version`, `-v` - Выводит на консоль версию компилятора.

`--watch`, `-w` - Наблюдатель отслеживающий измененные входные файлы.

### Модули

`--allowArbitraryExtensions` - Разрешить импорт файлов с любым расширением при условии наличия файла объявления.
Тип: boolean
По умолчанию: false

`--allowImportingTsExtensions` - Разрешить импорт расширений файлов TypeScript. Требуется задать `--modireResolution bundler`  и `--noEmit` или `--emitConfliction Only`.
Тип: boolean
По умолчанию: false

`--allowUmdGlobalAccess` - Разрешить доступ к глобальным UMD из модулей.
Тип: boolean
По умолчанию: false

`--baseUrl` - Укажите базовый каталог для разрешения имен не относительных модулей.

`--customConditions` - Условия, устанавливаемые в дополнение к значениям по умолчанию, определенным для распознавания при разрешении импорта.

`--module`, `-m` - Укажите, какой код модуля создается.
одно из: none, commonjs, amd, umd, system, es6/es2015, es2020, es2022, esnext, node16, nodenext
По умолчанию: undefined

`--moduleResolution` - Укажите, как TypeScript ищет файл из заданного спецификатора модуля.
одно из: classic, node10/node, node16, nodenext, bundler
По умолчанию: module === `AMD` или `UMD` или `System` или `ES6`, тогда `Classic`, иначе `Node`

`--moduleSuffixes` - Список суффиксов имен файлов для поиска при разрешении модуля.

`--noResolve` - Запретить `import's`, `require` или `<reference>` расширять число файлов, которые TypeScript должен добавлять в проект.
Тип: boolean
По умолчанию: false

`--paths` - Укажите набор записей, которые повторно сопоставляют импорт с дополнительными расположениями поиска.
По умолчанию: undefined

`--resolveJsonModule` - Включить импорт файлов .json.
Тип: boolean
По умолчанию: false

`--resolvePackageJsonExports` - При разрешении экспорта пакетов используйте поле package.json.
Тип: boolean
По умолчанию: `true` когда `moduleResolution` является `node16`, `nodenext`, или `bundler`, иначе `false`.

`--resolvePackageJsonImports` - При разрешении импорта используйте поле package.json.
Тип: boolean
По умолчанию: `true` когда `moduleResolution` является `node16`, `nodenext`, или `bundler`, иначе `false`.

`--rootDir` - Укажите корневую папку исходных файлов.
Тип: string
По умолчанию: Вычисляется по списку входных файлов.

`--rootDirs` - Разрешить рассматривать несколько папок как одну при разрешении модулей.
один или несколько: string
По умолчанию: Вычисляется по списку входных файлов

`--typeRoots` - Укажите несколько папок, которые действуют как `./node_modules/@types`.

`--types` - Укажите имена пакетов типов для включения без ссылок в исходный файл.

### Поддержка JavaScript

`--allowJs` - Разрешить участие файлов JavaScript в программе. Чтобы получить ошибки из этих файлов, воспользуйтесь опцией `startJS`.
Тип: boolean
По умолчанию: false

`--checkJs` - Включить отчеты об ошибках в файлах JavaScript с проверкой типа.
Тип: boolean
По умолчанию: false

`--maxNodeModuleJsDepth` - Укажите максимальную глубину папки, используемую для проверки файлов JavaScript из `node_modules`. Применимо только с `allowJs`.
Тип: number
По умолчанию: 0

### Зависимости взаимодействия

`--allowSyntheticDefaultImports` - Разрешить `import x from y`, если модуль не имеет экспорта по умолчанию.
Тип: boolean
По умолчанию: module === "system" или esModuleInterop

`--esModuleInterop` - Используйте дополнительный JavaScript для упрощения поддержки импорта модулей CommonJS. При этом для совместимости типов включен параметр allowExingDefureImports.
Тип: boolean
По умолчанию: false

`--forceConsistentCasingInFileNames` - Убедитесь в правильности окружения при импорте.
Тип: boolean
По умолчанию: true

`--isolatedModules` - Убедитесь, что каждый файл можно безопасно перенести, не полагаясь на другой импорт.
Тип: boolean
По умолчанию: false

`--preserveSymlinks` - Отключите разрешение символьных ссылок для их реальных путей. Это коррелирует с одним и тем же флагом в узле.
Тип: boolean
По умолчанию: false

`--verbatimModuleSyntax` - Не преобразовывать и не устранять импорт и экспорт, не помеченные как только тип, обеспечивая их запись в формате выходного файла на основе параметра `module`.
Тип: boolean
По умолчанию: false

### Контроль соответствия типов

`--allowUnreachableCode` - Отключить отчеты об ошибках для недоступного кода.
Тип: boolean
По умолчанию: undefined

`--allowUnusedLabels` - Отключить отчеты об ошибках для неиспользуемых меток.
Тип: boolean
По умолчанию: undefined

`--alwaysStrict` - Убедиться, что «use strict» всегда добавляется.
Тип: boolean
По умолчанию: `false`, если `strict` установлено

`--exactOptionalPropertyTypes` - Интерпретировать необязательные типы свойств как записанно, а не добавлять undefined.
Тип: boolean
По умолчанию: false

`--noFallthroughCasesInSwitch` - Включение отчетов об ошибках для случаев отказа в операторах switch.
Тип: boolean
По умолчанию: false

`--noImplicitAny` - Включить отчеты об ошибках для выражений и объявлений с подразумеваемым типом «any».
Тип: boolean
По умолчанию: `false`, если `strict` установлено

`--noImplicitOverride` - Убедиться, что переопределяющие элементы в производных классах помечены модификатором переопределения.
Тип: boolean
По умолчанию: false

`--noImplicitReturns` - Включение отчетов об ошибках для кодовых шаблонов, которые явно не возвращаются в функции.
Тип: boolean
По умолчанию: false

`--noImplicitThis` - Включить в отчеты об ошибках, когда «this» имеет тип «any».
Тип: boolean
По умолчанию: `false`, если `strict` установлено

`--noPropertyAccessFromIndexSignature` - Принудительно использует индексированные средства доступа для ключей, объявленных с использованием индексированного типа.
Тип: boolean
По умолчанию: false

`--noUncheckedIndexedAccess` - Добавить undefined к типу при обращении с помощью индекса.
Тип: boolean
По умолчанию: false

`--noUnusedLocals` - Включение отчетов об ошибках, когда локальные переменные не читаются.
Тип: boolean
По умолчанию: false

`--noUnusedParameters` - Создать ошибку, если параметр функции не прочитан.
Тип: boolean
По умолчанию: false

`--strict` - Включить все параметры строгой проверки типа.
Тип: boolean
По умолчанию: false

`--strictBindCallApply` - Убедиться, что аргументы для методов «bind», «call» и «apply» соответствуют исходной функции.
Тип: boolean
По умолчанию: `false`, если `strict` установлено

`--strictFunctionTypes` - При назначении функций проверяет, совместимы ли параметры и возвращаемые значения с подтипами.
Тип: boolean
По умолчанию: `false`, если `strict` установлено

`--strictNullChecks` - При проверке типа следует учитывать значения null и undefined.
Тип: boolean
По умолчанию: `false`, если `strict` установлено

`--strictPropertyInitialization` - Проверяет свойства класса, которые объявлены, но не заданы в конструкторе.
Тип: boolean
По умолчанию: `false`, если `strict` установлено

`--useUnknownInCatchVariables` - Переменные предложения catch по умолчанию «unknown» вместо «any».
Тип: boolean
По умолчанию: false

### Режимы наблюдения и построения

`--assumeChangesOnlyAffectDirectDependencies` - При повторной компиляции в проектах, использующих режим «incremental» и «watch», предполагается, что изменения в файле повлияют только на файлы непосредственно в зависимостях от него.
Тип: boolean
По умолчанию: false

### Обратная совместимость

`--charset` - Больше не поддерживается. В ранних версиях вручную устанавливается кодировка текста для чтения файлов.
Тип: string
По умолчанию: utf8

`--keyofStringsOnly` - Вместо строки, чисел или символов используйте только возвращаемые строки keyof. Устаревший вариант.
Тип: boolean
По умолчанию: false

`--noImplicitUseStrict` - Отключает добавление директив 'use strict' в скомпилированные файлы JavaScript.
Тип: boolean
По умолчанию: false

`--noStrictGenericChecks` - Отключает строгую проверку общих сигнатур в типах функций.
Тип: boolean
По умолчанию: false

`--out` - Устаревший параметр. Вместо этого используйте `outFile`.

`--suppressExcessPropertyErrors` - Отключение отчетов об ошибках избыточных свойств при создании литералов объектов.
Тип: boolean
По умолчанию: false

`--suppressImplicitAnyIndexErrors` - Подавление ошибок `noImplicitAny` при индексировании объектов, не имеющих сигнатур индекса.
Тип: boolean
По умолчанию: false

### Проект

`--composite` - Включить ограничения, позволяющие использовать проект TypeScript со ссылками на проект.
Тип: boolean
По умолчанию: false

`--disableReferencedProjectLoad` - Уменьшите число проектов, автоматически загружаемых TypeScript.
Тип: boolean
По умолчанию: false

`--disableSolutionSearching` - Исключение проекта из проверки многопроектных ссылок при редактировании.
Тип: boolean
По умолчанию: false

`--disableSourceOfProjectReferenceRedirect` - Отключите выбор исходных файлов вместо файлов объявлений при ссылке на составные проекты.
Тип: boolean
По умолчанию: false

`--incremental`, `-i` - Сохраните файлы `.tsbuildinfo`, чтобы обеспечить инкрементную компиляцию проектов.
Тип: boolean
По умолчанию: `false`, если `composite` установлено

`--tsBuildInfoFile` - Укажите путь к файлу инкрементной компиляции `.tsbuildinfo.`
Тип: string
По умолчанию: .tsbuildinfo

### Emit

`--declaration`, `-d` - Создание файлов .d.ts из файлов TypeScript и JavaScript в проекте.
Тип: boolean
По умолчанию: `false`, если `composite` установлено

`--declarationDir` - Укажите папку вывода для созданных файлов объявлений.

`--declarationMap` - Создание исходных схем для файлов d.ts.
Тип: boolean
По умолчанию: false

`--downlevelIteration` - Более совместимый, но подробный и менее эффективный JavaScript для итерации.
Тип: boolean
По умолчанию: false

`--emitBOM` - В начале выходных файлов ввести метку байтового порядка (BOM) UTF-8.
Тип: boolean
По умолчанию: false

`--emitDeclarationOnly` - Выводить только файлы d.ts, а не JavaScript.
Тип: boolean
По умолчанию: false

`--importHelpers` - Разрешить импорт вспомогательных функций из tslib один раз в проект, а не включать их в файл.
Тип: boolean
По умолчанию: false

`--importsNotUsedAsValues` - Укажите поведение emit/check для импорта, используемого только для типов.
один из: remove, preserve, error
По умолчанию: remove

`--inlineSourceMap` - Включить файлы sourcemap в выданный JavaScript.
Тип: boolean
По умолчанию: false

`--inlineSources` - Включить исходный код в исходные копии в выданном JavaScript.
Тип: boolean
По умолчанию: false

`--mapRoot` - Указать расположение, в котором отладчик должен находить файлы сопоставления вместо созданных расположений.

`--newLine` - Установить символ новой строки для создания файлов.
один из: crlf, lf

`--noEmit` - Отключить передачу файлов из компиляции.
Тип: boolean
По умолчанию: false

`--noEmitHelpers` - Отключить создание пользовательских вспомогательных функций, таких как '__ extensions' в скомпилированных выходных данных.
Тип: boolean
По умолчанию: false

`--noEmitOnError` - Отключить передачу файлов при появлении сообщений об ошибках проверки типа.
Тип: boolean
По умолчанию: false

`--outDir` - Укажите папку вывода для всех скомпилированных файлов.

`--outFile` - Укажите файл, объединяющий все выходные данные в один файл JavaScript. Если «declaration» имеет значение true, также обозначает файл, объединяющий все выходные данные .d.ts.

`--preserveConstEnums` - Отключить удаление объявлений const enum в сгенерированном коде.
Тип: boolean
По умолчанию: false

`--preserveValueImports` - Сохранение неиспользуемых импортированных значений в выходных данных JavaScript, которые в противном случае были бы удалены.
Тип: boolean
По умолчанию: false

`--removeComments` - Отключить компиляцию комментариев.
Тип: boolean
По умолчанию: false

`--sourceMap` - Создайть файлы карт источников для скомпилированных файлов JavaScript.
Тип: boolean
По умолчанию: false

`--sourceRoot` - Указывает корневой путь для отладчиков, чтобы найти исходный код ссылки.

`--stripInternal` - Отключить в своих комментариях JSDoc объявления `@internal`.
Тип: boolean
По умолчанию: false

### Диагностика компилятора

`--diagnostics` - Вывод информации о производительности компилятора после построения.
Тип: boolean
По умолчанию: false

`--explainFiles` - Печать файлов, прочитанных во время компиляции, включая причины ее включения.
Тип: boolean
По умолчанию: false

`--extendedDiagnostics` - Вывод более подробной информации о производительности компилятора после создания.
Тип: boolean
По умолчанию: false

`--generateCpuProfile` - Создание профиля CPU v8 компилятора, запускаемого для отладки.
Тип: string
По умолчанию: profile.cpuprofile

`--generateTrace` - Создает трассировку событий и список типов.

`--listEmittedFiles` - Печать имен выданных файлов после компиляции.
Тип: boolean
По умолчанию: false

`--listFiles` - Печать всех файлов, прочитанных во время компиляции.
Тип: boolean
По умолчанию: false

`--traceResolution` - Пути к журналу, используемые в процессе `modureResolution`.
Тип: boolean
По умолчанию: false

### Поддержка редактора

`--disableSizeLimit` - Удалите ограничение 20 МБ общего размера исходного кода для файлов JavaScript на сервере языка TypeScript.
Тип: boolean
По умолчанию: false

`--plugins` - Укажите список подключаемых модулей языковых служб.
один или несколько: 
По умолчанию: undefined

### Язык и среда

`--emitDecoratorMetadata` -  Метаданные типа конструкции для декорированных объявлений в исходных файлах.
Тип: boolean
По умолчанию: false

`--experimentalDecorators` - Включить экспериментальную поддержку устаревших экспериментальных декораторов.
Тип: boolean
По умолчанию: false

`--jsx` - Укажите, какой код JSX создается.
одно из: preserve, react, react-native, react-jsx, react-jsxdev
По умолчанию: undefined

`--jsxFactory` - Укажите функцию фабрику JSX, используемую при нацеливании React JSX emit, например React.createElement или h.
Тип: string
По умолчанию: `React.createElement`

`--jsxFragmentFactory` - Укажите ссылку JSX Fragment, используемую для фрагментов при нацеливании на React JSX, например React.Fragment или Fragment.
Тип: string
По умолчанию: React.Fragment

`--jsxImportSource` - Укажите спецификатор модуля, используемый для импорта фабричных функций JSX при использовании `jsx: react-jsx *`.
Тип: string
По умолчанию: react

`--lib` - Укажите набор пакетных файлов объявлений библиотек, описывающих целевую среду выполнения.
одно или несколько: es5, es6/es2015, es7/es2016, es2017, es2018, es2019, es2020, es2021, es2022, es2023, esnext, dom, dom.iterable, webworker, webworker.importscripts, webworker.iterable, scripthost, es2015.core, es2015.collection, es2015.generator, es2015.iterable, es2015.promise, es2015.proxy, es2015.reflect, es2015.symbol, es2015.symbol.wellknown, es2016.array.include, es2017.object, es2017.sharedmemory, es2017.string, es2017.intl, es2017.typedarrays, es2018.asyncgenerator, es2018.asynciterable/esnext.asynciterable, es2018.intl, es2018.promise, es2018.regexp, es2019.array, es2019.object, es2019.string, es2019.symbol/esnext.symbol, es2019.intl, es2020.bigint/esnext.bigint, es2020.date, es2020.promise, es2020.sharedmemory, es2020.string, es2020.symbol.wellknown, es2020.intl, es2020.number, es2021.promise/esnext.promise, es2021.string, es2021.weakref/esnext.weakref, es2021.intl, es2022.array, es2022.error, es2022.intl, es2022.object, es2022.sharedmemory, es2022.string/esnext.string, es2022.regexp, es2023.array/esnext.array, esnext.intl, decorators, decorators.legacy
По умолчанию: undefined

`--moduleDetection` - Управление методом обнаружения файлов JS модульного формата.
один из: legacy, auto, force
По умолчанию: «auto»: обработка файлов с импортом, экспортом, `import.meta`, `jsx` (с jsx: react-jsx) или форматом esm (с модулем: node16 +) в качестве модулей.

`--noLib` - Отключает включение любых файлов библиотеки, включая файл lib.d.ts. по умолчанию.
Тип: boolean
По умолчанию: false

`--reactNamespace` - Укажите объект, вызываемый для `createElement`. Это применимо только в том случае, если компилируются JSX.
Тип: string
По умолчанию: `React`

`--target`, `-t` - Установить версию языка JavaScript для скомпилированного JavaScript и включить совместимые объявления библиотеки.
одно из: es3, es5, es6/es2015, es2016, es2017, es2018, es2019, es2020, es2021, es2022, esnext
По умолчанию: es5

`--useDefineForClassFields` - Поля класса, совместимые со стандартом ECMAScript.
Тип: boolean
По умолчанию: true для ES2022 и выше, включая ESNext.

### Форматирование выходных данных

`--noErrorTruncation` - Отключить типы усечения в сообщениях об ошибках.
Тип: boolean
По умолчанию: false

`--preserveWatchOutput` - Отключить очистку консоли в режиме наблюдения.
Тип: boolean
По умолчанию: false

`--pretty` - Включить цвет и форматирование в выходных данных TypeScript, чтобы упростить чтение ошибок компилятора.
Тип: boolean
По умолчанию: true

### Прочее

`--skipDefaultLibCheck` - Пропустить проверку типа файлов .d.ts, включенных в TypeScript.
Тип: boolean
По умолчанию: false

`--skipLibCheck` - Пропустить проверку типа всех файлов .d.ts.
Тип: boolean
По умолчанию: false

Все параметры компилятора можно узнать по адресу https://aka.ms/tsc

### Варианты наблюдения

Включая --watch, -w начнётся наблюдение в текущем проекте за изменениями файлов. После установки режим наблюдения можно сконфигурировать следующим образом:

`--watchFile` - Указать, как работает режим наблюдения в TypeScript.
одно из: fixedpollinginterval, prioritypollinginterval, dynamicprioritypolling, fixedchunksizepolling, usefsevents, usefseventsonparentdirectory
По умолчанию: usefsevents

`--watchDirectory` - Указать, как осуществляется наблюдение каталогов в системах, в которых отсутствует функция рекурсивного просмотра файлов.
одно из: usefsevents, fixedpollinginterval, dynamicprioritypolling, fixedchunksizepolling
По умолчанию: usefsevents

`--fallbackPolling` - Указать, какой подход следует использовать наблюдателю, если система не работает с наблюдателями за собственными файлами.
одно из: fixedinterval, priorityinterval, dynamicpriority, fixedchunksize
По умолчанию: priorityinterval

`--synchronousWatchDirectory` - Синхронный обратный вызов и обновление состояния наблюдателей за каталогами на платформах, которые не поддерживают рекурсивное наблюдение.
Тип: boolean
По умолчанию: false

`--excludeDirectories` - Удаление списка каталогов из процесса наблюдения.

`--excludeFiles` - Удаление списка файлов из режима наблюдения.

### Варианты компиляции

Используя --build, -b заставит tsc управлять  компиляцией проекта. Это используется для инициирования создания составных проектов, о которых вы можете узнать больше на https://aka.ms/tsc-composite-builds

`--verbose`, `-v` - Включить детальное ведение журнала.

`--dry`, `-d` - Показать, что будет создано (или удалено, если указано с `--clean`).

`--force`, `-f` - Компиляция всех проектов, включая проекты, которые, как представляется, являются актуальными.

`--clean` - Удаление выходных данных всех проектов.


