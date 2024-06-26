# Размеры элементов

## Размеры в процентах и долях

Можно указывать в абсолютных значениях. Например, 300px.

Можно указать в процентах от родительского элемента. Например, 30%.

Можно указывать в сравнении с окном просмотра (`viewport`, размер браузерного окна), например: 50vh — половина высоты, 50vw — половина ширины.

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

Зачастую браузеры своеобразно интерпретируют размеры элемента (например, при использовании `float`). В частности, у всех элементов по умолчанию для свойства `box-sizing` используется значение `content-box`, то есть при определении ширины и высоты элемента браузер будет прибавлять к значению свойств `width` и `height` также и внутренние отступы `padding` и ширину границы. В итоге это может привести к выпадению плавающих элементов из тех блоков, которые для них предназначены. Поэтому часто для всех элементов рекомендуется устанавливать для свойства `box-sizing` значение `border-box`, чтобы все элементы измерялись одинаково, а их ширина представляла только значение свойства `width`.&#x20;
