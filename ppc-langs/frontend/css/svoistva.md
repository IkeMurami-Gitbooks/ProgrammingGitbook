# Свойства

## Цвет

```css
div {
    background-color: red;  // Цвет фона
    color: red;             // Цвет текста
    border-color: red;      // Цвет границы
    opacity: 0.4;           // Прозрачность
}
```

Полный перечень цветов: [https://developer.mozilla.org/en-US/docs/Web/CSS/color\_value](https://developer.mozilla.org/en-US/docs/Web/CSS/color\_value)

## Шрифты

```css
p {
    font-style: italic;
    font-weight: 100 bold;
    color: red;
    font-family: "Times New Roman", Arial, sans-serif, serif;
    font-size: 18px;
}
```

## Стилизация абзацев

```css
p {
    line-height: 150%; // межстрочный интервал
    text-align: left; // выравнивание текста относительно одной из сторон страницы
    text-indent: 35px;  // отступ первой строки абзаца
}
```

## Блочная модель и отступы

Для веб-браузера элементы страницы представляют небольшие контейнеры или блоки. Такие блоки могут иметь различное содержимое - текст, изображения, списки, таблицы и другие элементы. Внутренние элементы блоков сами выступают в качестве блоков.

Схематично блочную модель можно представить следующим образом:

![](<../../../.gitbook/assets/изображение (10).png>)

Отступы могут иметь значения в пикселях, em (значения относительно шрифта элемента), процентных соотношениях (относительно ширины элемента-контейнера), либо auto (автоматическая установка отступов).

## Размеры элементов

```css
div {
    width: 150px;
    width: 75%;
    height: 15em;
    
    min-width: 1px;
    max-width: 1px;
    min-height: 1px;
    max-height: 1px;
    box-sizing: border-box;  // box-sizing — указывает, как должны считаться размеры блока: с учетом или без margin, padding, border,..
    
}
```

Зачастую браузеры своеобразно интерпретируют размеры элемента (например, при использовании `float`). В частности, у всех элементов по умолчанию для свойства `box-sizing` используется значение `content-box`, то есть при определении ширины и высоты элемента браузер будет прибавлять к значению свойств `width` и `height` также и внутренние отступы `padding` и ширину границы. В итоге это может привести к выпадению плавающих элементов из тех блоков, которые для них предназначены. Поэтому часто для всех элементов рекомендуется устанавливать для свойства `box-sizing` значение `border-box`, чтобы все элементы измерялись одинаково, а их ширина представляла только значение свойства `width`. Поэтому нередко в стилях добавляется следующий стиль:

## Прокрутка элементов

Свойство overflow — добавляет возможность прокрутки:

```css
auto: если контент выходит за границы блока, то создается прокрутка. В остальных случаях полосы прокрутки не отображаются
hidden: отображается только видимая часть контента. Контент, который выходит за границы блока, не отображается, а полосы прокрутки не создаются
scroll: в блоке отображаются полосы прокрутки, даже если контент весь помещается в границах блока, и таких полос прокрутки не требуется
visible: значение по умолчанию, контент отображается, даже если он выходит за границы блока
```

С помощью дополнительных свойств `overflow-x` и `overflow-y` можно определить прокрутку соответственно по горизонтали и по вертикали.

## Rotation

rotate icons example: [https://codepen.io/colewaldrip/pen/bdZVGd/](https://codepen.io/colewaldrip/pen/bdZVGd/)

text rotation: [https://css-tricks.com/snippets/css/text-rotation/](https://css-tricks.com/snippets/css/text-rotation/)

Краткий пример переворота кнопки:

```markup
<div class="main-container">
  <button class="btn">
    <p>Button</p>
  </button>
  <div class="someelem"></div>
</div>
```

```css
.main-container {
  display: flex;
  height: 100%;

  .btn {
    background-color: lightcoral;
    border: 0;
    p {
      transform: rotate(-90deg);
    }
  }

  .someelem {
    width: 100%;
    background-color:cadetblue;
  }
}
```
