# Строчные и блочно-строчные элементы

Кроме блочных элементов, в HTML есть строчные. Например, ссылки.

Блочный элемент занимает по умолчанию всю ширину родительского элемента.

Строчные элементы занимают ровно столько места, сколько контента в них содержится. Им невозможно задать ширину или высоту — они игнорируют указание размеров через стили.

Что делать, когда блоки с определенными размерами должны следовать друг за другом по горизонтали и не занимать всю строку? Можно задать элементам комбинированный тип — **блочно-строчный**. С одной стороны, они не занимают собой всю горизонталь, с другой, восприимчивы к указанию размеров через CSS. Например, так ведут себя элементы `img`.

```css
display: block; /* сделает элемент блочным */
display: inline; /* сделает элемент строчным */
display: inline-block; /* сделает элемент блочно-строчным */ 
```

Еще элемент можно сделать flex-элементом. В этом случае появляются доп возможности (например, `margin: auto` центрирует и по вертикали и по горизонтали):

```css
display: flex;
```
