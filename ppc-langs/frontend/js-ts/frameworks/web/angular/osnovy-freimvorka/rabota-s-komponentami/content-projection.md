# Content Projection

## About

Это шаблон, в который вставляются данные из другого компонента. Например, у вас есть компонент Card, и в него попадают данные из вашего компонента.

Этот подход позволяет создавать переиспользуемые компоненты.

Реализация Content Projection в Angular:

* Single-slot content projection: компонент принимает контент из одного источника;
* Multi-slot content projection: компонент принимает контент из нескольких источников;
* Conditional content projection: компоненты отображают контент при определенных условиях.

Создается Content Projection через элемент-placeholder `<ng-content>`.

## Single-slot content projection

Наш Content Projection компонент — `<app-some-example>`:

```markup
<h2>Single-slot content projection</h2>
<ng-content></ng-content> <!-- Сюда будет вставляться контент -->
```

Использование Content Projection компонента в другом компоненте:

```markup
<app-some-example>
    <p>Content projection</p>
</app-some-example>
```

## Multi-slot content projection

Content Projection компонент `<app-some-example>`:

Angular поддерживает CSS-селекторы здесь

```markup
<h2>Multi-slot content projection</h2>

Default:
<ng-content></ng-content>

Question:
<ng-content select="[question]"></ng-content>
```

Использование:

```markup
<app-some-example>
  <p question>
    Content projection
  </p>
  <p>Let's learn about content projection!</p>
</app-some-example>
```

## Conditional content projection

Если контент должен отрисовываться в зависимости от каких-то условий (`ngIf`, например), то не рекомендуется использовать `<ng-content>`. Для этих целей существует `<ng-template>` и `<ng-container>`.

Пример смотри здесь: [https://angular.io/guide/content-projection](https://angular.io/guide/content-projection)
