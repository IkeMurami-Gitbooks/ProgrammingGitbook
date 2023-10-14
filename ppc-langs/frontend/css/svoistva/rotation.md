# Rotation

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
