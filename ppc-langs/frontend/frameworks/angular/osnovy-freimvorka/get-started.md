# Get Started

## Install

Angular CLI — это доп пакет, служащий для комфортной разработки на Angular. Устанавливаем Angular CLI:

```
$ npm install -g @angular/cli
```

Если писать в VS Code, установить расширение Angular Language Service.

## Usage

Create Project

```
$ ng new my-app
```

Run app (флаг open сразу откроет браузер)

```
$ cd my-app
$ ng serve --open
```

## File Structure

### Workspace

| Файл                | Назначение                                                                                 |
| ------------------- | ------------------------------------------------------------------------------------------ |
| `.editorconfig`     | Системный файл, который содержит настройки для разных редакторов                           |
| `.gitignore`        | Файлы и папки, которые не будут добавляться в репозиторий                                  |
| `README.md`         |                                                                                            |
| `angular.json`      | Конфигурационный файл Angular приложения                                                   |
| `package.json`      | Список зависимостей + набор скриптов для автоматизации запуска нашего приложения           |
| `package-lock.json` | Конфигурационный файл, который служит для более эффективной работы с версионностью пакетов |
| `e2e/`              | Код для end-to-end тестированиия (с protractor)                                            |
| `src/`              | Исходный код приложения                                                                    |
| `node_modules/`     | Зависимости приложения (набор библиотек)                                                   |
| `tsconfig.json`     |                                                                                            |
| `tslint.json`       |                                                                                            |
| `karma.conf.js`     | конфиг для karma — инструмента для написания и запуска Unit-тестов                         |
| `browserslist`      | Требования к браузерам, в которых должно работать приложение                               |

### angular.json

`sourceRoot` — путь до папки с исходны кодом

`options/prefix` — префикс для компонентов (например, `prefix = app` =>  `app-root`)

`options/tsConfig` — путь до tsconfig-файла

`options/styles` — путь до стилей (например, сюда будут подключаться Materialize, Bootstrap, ..)

`configurations/production/fileReplacements` — заменяет конфиг в зависимости от типа сборки (dev, prod, ..)

Так же содержит настройки по оптимизации, e2e, lint, tests, ...

### package.json

Про команды (объявляются в конфиге):

start (`npm start`) — собирает приложение в режиме разработки

build (`npm build`) — собирает приложение в режиме production (можно еще добавить флаг, чтобы была максимальная оптимизация: `ng build --prod`)

### src/

`index.html` — содержит элемент `app-root`

`main.ts` — точка входа приложения, Все начинается с запуска какого-то модуля:

```typescript
import { platformBrowserDynamic } from '@angular/platform-browser-dynamic';

import { AppModule } from './app/app.module';

...
platformBrowserDynamic().bootstrapModule(AppModule)
...
```

`polyfills.ts` — добавление JS-кода, который будет запущен раньше нашего приложения. Используется для включения поддержки доп функциональности в разных браузерах.

`app/` — по-факту хранит код нашего модуля-приложения. (`app` — это сущность "модуль", `app.component` — это сущность "компонент").

Про название файлов в модуле:

```
<Название элемента>.<Тип элемента>[.spec.].<расширение>

app.component.html
app.module.ts
something.component.spec.ts
```

Все начинается с модуля (например, `app.module.ts`), где мы импортируем базовый модуль в массив imports, регистрируем компонент в declarations:

```typescript
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';

import { AppComponent } from './app.component';

@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

