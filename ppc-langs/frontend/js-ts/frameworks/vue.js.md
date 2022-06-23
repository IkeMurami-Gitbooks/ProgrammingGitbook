# Vue.js

## Create and build Vue.js project

```
// Установка vue.js и запуск команды vue-create
$ npm init vue@latest
// Установка зависимостей и запуск dev стенда
$ cd vue-project
$ npm install
$ npm run dev
```



## Структура проекта

```
.
├── app.css
├── App.vue
├── assets
│   └── ...
├── components
│   └── ...
├── main.js
├── mixins
│   └── ...
├── router
│   └── index.js
├── store
│   ├── index.js
│   ├── modules
│   │   └── ...
│   └── mutation-types.js
├── translations
│   └── index.js
├── utils
│   └── ...
└── views
    └── ...
```

assets - любые ассеты, импортируемые в компоненты \
components - все компоненты, за исключением главного view \
mixins - часть js кода, используемого в нескольких компонентах \
router - все пути в приложении: н-р, /dashboard, /search \
store (optional) - \
translations (optional) - локаль \
utils (optional) \
views -

