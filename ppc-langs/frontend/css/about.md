# About

## Стили

Есть разные подходы к оформлению стилей:

* CSS
* CSS Modules
* SCSS [https://sass-lang.com/documentation/syntax#scss](https://sass-lang.com/documentation/syntax#scss)
* Sass [https://sass-lang.com/documentation/syntax#the-indented-syntax](https://sass-lang.com/documentation/syntax#the-indented-syntax)
* Less [http://lesscss.org](http://lesscss.org)

## Materials, Examples & Links

CSS Tricks — содержит хорошие примеры по flexbox, gridlayout, dark themes и многое другое: [https://css-tricks.com](https://css-tricks.com)

## Введение

CSS — отвечает за стили, отображение, анимацию и другой UI.

Используется, чтобы легче было разрабатывать html-документы, адаптировать их под свои нужды, сделать более живыми.

Пример на React. Создание класса `wrapper`:

```css
.wrapper {
    padding-top: 5rem;
    margin: 0 auto;
    width: 600px;
}
```

Использование класса в JSX:

```jsx
import React from 'react'

function App() {
    return <div className='wrapper' />
}

export default App
```

Отображаться это будет как:

```markup
<div class="wrapper" />
```

## Особенности SCSS

Поддерживает вложенные стили:

```css
.main-container {
    .second-container {
        height: 100%;
    }
}
```
