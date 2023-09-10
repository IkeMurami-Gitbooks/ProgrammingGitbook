# Basic

Приложение  состоит из 3-х частей:

* component-классы, описывают данные и функциональность
* шаблоны HTML, которые описывают UI
* CSS

html тэги (прочитать про [компоненты](https://angular.io/guide/architecture-components))

* `<app-root>` — первый компонент для загрузки и в которые загружаются другие компоненты
* `<app-top-bar>`, `<app-product-list>`, `<app-product-alerts>` — другие компоненты

## В Angular есть встроенные механизмы санитайзинга:&#x20;

```markup
<h3>Binding innerHTML</h3>
<p>Bound value:</p>
<p class="e2e-inner-html-interpolated">{{htmlSnippet}}</p>
<p>Result of binding to innerHTML:</p>
<p class="e2e-inner-html-bound" [innerHTML]="htmlSnippet"></p>
```

```javascript
export class InnerHtmlBindingComponent {
  // For example, a user/attacker-controlled value from a URL.
  htmlSnippet = 'Template <script>alert("0wned")</script> <b>Syntax</b>';
}
```

тэг скрипт будет выпилен Angular'ом при отображении

## DomSanitizer

При аудите кода, ищем вызовы этого класса. Эти методы отключают поддержку встроенных в Angular механизмов безопасности [https://angular.io/api/platform-browser/DomSanitizer](https://angular.io/api/platform-browser/DomSanitizer)

