# Init

TypeScript — статически типизированный JS. Под капотом компилируется в JavaScript.

site: [https://www.typescriptlang.org/](https://www.typescriptlang.org)

IDE online: [https://www.typescriptlang.org/play/?ssl=2\&ssc=26\&pln=2\&pc=48#](https://www.typescriptlang.org/play/?ssl=2\&ssc=26\&pln=2\&pc=48#)

Неплохой видеокурс на русскком языке: [https://youtube.com/playlist?list=PLqKQF2ojwm3nW-cQeSER79xdpK3vL5c-g](https://youtube.com/playlist?list=PLqKQF2ojwm3nW-cQeSER79xdpK3vL5c-g)

## Установка

Устанавливаем поддержку языка в систему глобально (чтобы можно было пользоваться возможностями typescript из консоли)

```
npm install -g typescript
```

## Создание и настройка проекта

### npm и typescript

Создание проекта:

```
$ npm init

или создания проекта по умолчанию:
$ npm init -y
```

Добавляем зависимости

```
$ npm install -D typescript
(-D — значит добавление зависимости для разработки, можно использовать --save-dev)

Как результат в package.json увидим в разделе devDependencies зависимость от typescript
```

### Структура проекта

Про настройку этого всего см&#x20;

```
./src — для ts-скриптов
./dist - для скомпиленных js-файлов
```

### tsconfig.json

Служит для настройки компилятора TypeScript.

Инициализация конфига (`tsconfig.json`) для typescript (чтобы каждый ts-скрипт не прогонять через tsc вручную, как описано далее в примере):

```
$ tsc --init
```

Интересные настройки

```javascript
{
    "compilerOptions": {}, // настройки компилятора
    "exclude": ["./test.ts", "./src/test2.ts"],  // Те файлы, которые мы захотим исключить на этапе компиляции
    "include": [
        "./src/**/*"  // Указываем какие файлы и директории хотим компилить
    ],
    "files": [
        "./app.ts" // Можно перечислять отдельные файлы
    ]
}
```

По умолчанию, в exclude входит директория node\_modules.  Ее отдельно указывать не надо.

Про `compilerOptions`:

```javascript
{
    "compilerOptions": {
        "target": "es6", // По умолчанию es5, но es6 уже работает почти во всех браузерах
        "module": "commonjs", // Используется при настройке webpack (пока хз это о чем)
        "outDir": "./dist",  // Куда класть скомпиленные js
        "rootDir": "./src",   // Где корневая директория с ts-кодом
        // "lib": [] // см ниже,
        // "allowJs": true,  // Использовать если мы ведем разработку на JS
        // "checkJs": true,  // и хотим использовать только некоторые фичи TS
        // "jsx": "preserve"  // При разработки на React совместно с TS
        // "sourceMap": true,  // 
    }
}
```

#### Про lib

Разкомменчиваем `lib` в tsconfig.json.

Допустим в html добавляем кнопку:

```markup
<body>
    <div class="container">
        <button class="btn" id="btn">Click me pls</button>
    </div>
    
</body>
```

В typescript пробуем обратиться к ней:

```typescript
const btn = document.querySelector('#btn')
```

И TypeScript не понимает что за document. Это потому, что в typescript отключен доступ к Browser API (поле lib пустое). Настройка lib регулирует, какие дополнительные надстройки добавить в typescript.

Если lib закомменчино, то по умолчанию все надстройки включаются (то есть у нас таких проблем не будет). Зачем это нужно? Для более тонкой настройки того что может и не может наш скрипт.

## Простой пример

Создаем файл `test.ts`:

```typescript
const str: string = 'Hello'

console.log(str)
```

Компилируем через компилятор TypeScript (рядом будет лежать test.js по результату):&#x20;

```
$ tsc test.st

Скомпилить все файлы (конфигурируется tsconfig.json):
$ tsc

Чтобы каждый раз не вводить tsc, можно сказать компилятору, чтобы он отслеживал состояние проекта.
В этом случае, все изменение будут сразу скомпилены:
$ tsc --watch
или
$ tsc -w
```

Запускаем, например, через NodeJS (надо установить [https://nodejs.org/en/](https://nodejs.org/en/)):

```
$ node test.js
```

Подгрузка в html. Атрибут `defer` откладывает выполнение скрипта до тех пор, пока вся страница не будет загружена полностью.:

```markup
<!doctype html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport"
          content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>TypeScript</title>
    <!-- Compiled and minified CSS -->
    
    <script src="dist/app.js" defer></script>
</head>
<body>
    <div class="container">
    </div>
</body>
</html>

```
