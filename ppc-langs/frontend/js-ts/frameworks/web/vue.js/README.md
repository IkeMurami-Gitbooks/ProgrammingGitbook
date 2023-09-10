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

## Security

### CSTI / SSTI

В приложениях Vue.js присутствует возможность использовать шаблоны, как и в Angular, через `{{}}`. Больше об эксплуатации CSTI, полезных нагрузках и обходе WAF и фильтров, используя особенности Vue.js можно [прочесть](https://portswigger.net/research/evading-defences-using-vuejs-script-gadgets) в блоге PortSwigger.

Пример data binding'а:

```markup
<template>
  <div id="csti">
    <h1>AAAA</h1>
    <input v-model="message" placeholder="edit me">
    <p>Message out: {{ message }}</p>
    <h1>BBBB</h1>
  </div>
</template>

<script>
export default {
  data() {
    return {
      message: ''
    }
  }
}
</script>
```

По умолчанию этот механизм защищен (автоматически кодируются данные), но в случае CSTI или отключения кодирования (через атрибут `v-html`) получаем XSS. Соотв, через этот атрибут мы можем попытаться выкрутить html injection в xss.

### Использование Render Functions

Render Function:

```typescript
h('div', {
  innerHTML: this.userProvidedHtml
})
```

Render function with JSX:

```html
<div innerHTML={this.userProvidedHtml}></div>
```

### Injecting URLs

```markup
<a :href="userProvidedUrl">
  click me
</a>

PAYLOAD = "javascript:"
```

В качестве Mitigation поможет [sanitize-url](https://www.npmjs.com/package/@braintree/sanitize-url). Но санитизация должна проходитить на стороне сервера.

### Никогда не используем непроверенные данные в templates

```typescript
Vue.createApp({
  template: `<div>` + userProvidedString + `</div>` // NEVER DO THIS
}).mount('#app')
```

### Injecting Styles

```markup
<a
  :href="sanitizedUrl"
  :style="userProvidedStyles"
>
  click me
</a>
```

Небезопасно давать возможность инжектиться в userProvidedStyles, тк это все равно, что такой код:

```markup
<style>{{ userProvidedStyles }}</style>
```

Правильно давать доступ только к отдельным полям:

```markup
<a
  :href="sanitizedUrl"
  :style="{
    color: userProvidedColor,
    background: userProvidedBackground
  }"
>
  click me
</a>
```

### Injecting JavaScript

Все элементы \<script> не будут отрендерены Vue.js, но надо быть внимательными при инжекте параметров в функции onclick, onfocus, onmouseenter, ... Инжект в эти атрибуты несет потенциальный риск, и этого стоит избегать.

### Server-Side Rendering (SSR)

По умолчанию Vue.js рендерит все в браузере, но есть возможность [производить](https://vuejs.org/guide/scaling-up/ssr.html) рендеринг форм на стороне сервера.

Ключевые слова для поиска по коду:

```typescript
import { createSSRApp } from 'vue'
import { renderToString } from 'vue/server-renderer'
```

Существуют верхнеуровневые решения для SSR: Nuxt, Quasar, Vite SSR.
