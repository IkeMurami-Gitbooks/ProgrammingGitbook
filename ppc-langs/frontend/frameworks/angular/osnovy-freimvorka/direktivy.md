# Directives

Директивы — в Angular это некоторые вспомогательные методы, которые можно использовать внутри шаблона. Можно создавать свои директивы.

## Директивы стиля

Это директивы, которые позволяют нам менять стили элементов и компонентов.

### ngStyle

директива позволяет динамически изменять стиль элемента

```markup
<div class="card">
    <h2 [ngStyle]="{
        color: title.length < 5 ? 'black' : 'green'
    }">{{ title }}</h2>
</div>
```

### ngClass

Позволяет управлять классом элемента динамически

```markup
<div class="text" [ngClass]="{
    blue: textColor === 'blue',
    red: textColor === 'red'
}"></div>

<!-- или -->

<div class="text" 
    [class.blue]="textColor === 'blue'"
    [class.red]="textColor === 'red'"
></div>
```

## Структурные директивы

Это директивы, которые позволяют менять содержание html. Символ `*` означает, что директива меняет html.

### ngIf

Добавлять или не добавлять компоненты в html.

```markup
<div *ngIf="toggle">
    <app-card></app-card>
    <app-card></app-card>
    <app-card></app-card>
    <app-card></app-card>
</div>
```

директива Else для ngIf:

```markup
<div *ngIf="toggle; else noCards">
    <app-card></app-card>
    <app-card></app-card>
    <app-card></app-card>
    <app-card></app-card>
</div>

<ng-template #noCards>
    <p>Cards are hidden</p>
</ng-template>
```

### ngFor

Эта директива позволяет работать с коллекцией элементов. Пример ее использования:

```markup
<div class="container">
    <!-- Example 1 -->
    <app-card
        *ngFor="let iterCard of cards"
        [card]="iterCard"
    ></app-card>
    
    <!-- Example 2 -->
    <h2>Products:</h2>
    <div *ngFor="let product of products">
        <h3>
            {{ product.name }}
        </h3>
    </div>
</div>
```

через атрибут `[card]` мы связываем объект `iterCard` с компонентом `CardComponent` (см далее)

В коде компонента формы (является родительской для компонента карты):

```typescript
export interface Card {
    title: string 
}


@Component({
    selector: 'app-form',
    ...
})
export class FormComponent {
    
    cards: Card[] = [
        {title: 'Card 1'},
        {title: 'Card 2'}
    ]    
    
}
```

В компоненте карты добавляем доступ до полей объекта card из родительского компонента FormComponent:

```typescript
import { Component, Input } from '@angular/core';
import { Card } From '../app.component';

@Component({
    selector: 'app-card',
    ...
})
export class CardComponent {
    
    @Input() card: Card
    @Input() index1: number
}
```

Получени индексов из цикла `ngFor` (`index` — это системное название):

```markup
<div class="container">
    <app-card
        *ngFor="let iterCard of cards; let idx = index"
        [card]="iterCard"
        [index]="idx + 1"
    ></app-card>
</div>
```
