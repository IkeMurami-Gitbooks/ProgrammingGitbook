# Выровнять элемент по центру страницы

![](<../../../../.gitbook/assets/изображение (9).png>)

Через таблицу:

```css
.begin-container {
  background: coral;
  padding: 5px;

  width: 100%;
  height: 100%;

  display: table;
  position: absolute;

  > .inner-container {
    display: table-cell;
    text-align: center;
    vertical-align: middle;
  }

  button {
    height: auto;
    display: inline-block;
    margin: 5px;
  }
}

```

```markup
<div class="begin-container">

  <div class="inner-container">
    <!-- Кнопки будут по центру -->
    <button mat-raised-button color="primary">New Project</button>
    <button mat-raised-button color="primary">Import Project</button>

  </div>

</div>
```

Другие варианты: [https://habr.com/ru/post/238449/](https://habr.com/ru/post/238449/)
