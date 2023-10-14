# Цвет

## Предустановленные цвета

```css
div {
    background-color: red;  // Цвет фона
    color: red;             // Цвет текста
    border-color: red;      // Цвет границы
    opacity: 0.4;           // Прозрачность
}
```

Полный перечень цветов: [https://developer.mozilla.org/en-US/docs/Web/CSS/color\_value](https://developer.mozilla.org/en-US/docs/Web/CSS/color\_value)

## RGB палитра

```css
div {
    color: rgb(0, 255, 255);
}
```

## HEX цвета

```css
div {
    color: #343434;
}
```

## Прозрачность

Цвет можно сделать прозрачным, дополнив RGB-палитру альфа-каналом, который устанавливает прозрачность.

```css
/* один и тот же цвет */
rgba(115, 170, 200, 0.5);
rgba(115, 170, 200, .5);
```

Рекомендуется писать без 0 (то есть второй вариант).
