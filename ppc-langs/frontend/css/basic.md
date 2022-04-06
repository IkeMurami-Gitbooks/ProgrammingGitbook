# Basic

Стили могут применяться к классу, к элементу.

Применение к классу card (`<div class="card"></div>`):

```css
.card {
    padding: .5rem 1rem;
}
```

Применение ко всем элементам h2 в классе card:

```css
.card {
    h2 {
        margin-bottom: .5rem;
    }
}
```

Ко всем классам text в классе card:

```css
.card {
    .text {
        padding: .5rem 1rem;
    }
}
```

Условие применения стиля, чтобы у элемента было два класса:

```css
.card {
    .text {
        .padding: .5rem 1rem;
        
        &.red {
            color: red;
        }
    }
}
```

Тогда, если элемент имеет класс class="text red", то к нему применятся оба стиля

**CSS поддерживает переменные.**
