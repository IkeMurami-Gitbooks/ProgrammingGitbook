# Объявление и приоритеты

## Приоритеты стилей

```markup
<div class="someclass" id="someid"></div>
```

id имеет наивысший приоритет, потом class, потом свойства самого элемента div

```css
#someid { }
.someclass { }
div { }
```

## Объявление стилей

Универсальный селектор:

```css
* {  // Универсальный селектор — применяет стили ко всем элементам html-документа
    background-color: red;
}
```

Применение стилизации к группе элементов:

```css
h1, h2, .someclass {  // применение стилизации к группе элементов
    text-align: center;
}
```

### Селекторы атрибутов

```css
#someid[src="data.js"] {
    color: red;
}
```

Регулярные выражения в селекторе атрибутов:

```css
#someid[href^="https://"] {}  // начинается с https://
#someid[href$=".jpg"] {} // Заканчивается на .jpg
#someid[href*="image"] {} // Содержит image в значении атрибута
```

### Применение стилей к потомкам

Потомки — все вложенные элементы (с любой глубиной вложенности)

#### Применение ко всем потомкам

```markup
<div class="one">
    <p>Test</p>
</div>
<div class="two">
    <p>Test</p>
</div>
```

```css
.one p {
    color: red;
}
.two p {
    color: coral;
}
```

**Важно**: пробел здесь играет важную роль. Есть написать так:

```css
div.one {
    color:red;
}
```

То стиль применется ко всем `div`-элементам, у которых указан класс `one`, например:

```markup
<div class="one"></div>
```

#### Применение стиля к дочернему элементу

Для `body` дочерним элементов является `div`, для `div` — `p`:

```markup
<body>
    <div class="one">
        <p>test</p>
    </div>
</body>
```

Применение стиля к дочернему элементу:

```css
div > p {
    color: red;
}
```

### Применение стилей к элементам одного уровня

```markup
<h2>Title</h2>
<div class="one"></div>
<div class="two"></div>
```

Применить стиль к элементу div, который идет непосредственно после h2:

```css
h2+div {
    color: red;
}
```

Применить стиль ко всем элементам div, которые находятся на одном уровне с элементом h2:

```css
h2~div {
    color: red;
}
```
