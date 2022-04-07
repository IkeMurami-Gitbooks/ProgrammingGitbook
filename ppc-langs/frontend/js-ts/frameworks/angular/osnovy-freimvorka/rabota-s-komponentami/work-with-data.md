# Component Interaction

## Pass data between HTML and TS

### Добавление динамики

Создаем в компоненте переменную title

```typescript
import {Component} from '@angular/core'

@Component({
  selector: 'app-card',
  templateUrl: './card.component.html',
  styleUrls: [
    './card.component.scss'
  ]
})
export class CardComponent {

  title = 'My Card Title'

}

```

Через интерполяцию добавляем эту перенную в наш шаблон:

```markup
<div class="card">
  <h2>{{ title }}</h2>
  <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit. Adipisci, ratione.</p>
</div>
```

Интерполировать можно все, что можно привести к строке — любые простые типы. Если мы попробуем проинтерполировать объект, то мы получим `[object Object]`. Чтобы все таки увидеть содержимое объекта можно использовать такую штуку как пайпы. Например, конвертнем объект в json:

```markup
<div class="card">
    <h2>{{ someObject | json }}</h2>
</div>
```

Через интерполирование можно вызывать методы (определенные в классе компонента)

В angular можно настроить символы для интерполяции:

```typescript
import {Component} from '@angular/core'

@Component({
  selector: 'app-card',
  templateUrl: './card.component.html',
  styleUrls: [
    './card.component.scss'
  ],
  interpolation: ['{{', '}}']
})
export class CardComponent {}
```

### Bindings

Это механизм связки компонента и шаблона, который присутствует в Angular. Позволяет в одностороннем порядке связывать данные (изменилось что-то в компоненте — изменилось во view).

Допустим, мы хотим менять динамически какой-то элемент шаблона (например, картинку).&#x20;

```typescript
import {Component, OnInit} from '@angular/core'

@Component({
  selector: 'app-card',
  templateUrl: './card.component.html',
  styleUrls: [
    './card.component.scss'
  ],
  interpolation: ['{{', '}}']
})
export class CardComponent implements OnInit {
  imgUrl: string = 'https://some.img/img.jpg'
  
  ngOnInit() {
    setTimeout(() => {
      this.imgUrl = 'https://some.img/img2.jpg'
    }, 3000)
  }
  
}
```

В html:

```markup
<div>
    <img src="{{ imgUrl }}">
</div>
```

Это сработает, но такой синтаксис не совсем корректен. Для этого есть механизм binding.

Делается это через оборачивание атрибута в квадратные скобки:

```markup
<div>
    <img [src]="imgUrl">
</div>
```

Соотв, Angualr понимает, что в значении атрибута будет код TS (или JS).

### Event binding (Two Way Binding)

Делается это через оборачивания события в круглые скобки и прописывания обработчика:

```markup

<div class="card">
  <h2>{{ title }}</h2>
  <p>{{ testText }}</p>

  <div>
    <button (click)="changeTitle()">Change Title</button>
    <button (click)="title = 'Inline title'">Change Title Inline</button>
  </div>
  
  <div>
    <!-- $event — это нативный ивент, нужен для обработчика -->
    <input type="text" (input)="inputHandler($event)" [value]="title">  
    
    <!-- Другой вариант (более простой) — через локальную ссылку на элемент -->
    <input type="text" #myInput (input)="inputHandler2(myInput.value)" [value]="title">
  </div>
</div>

```

В angular есть встроенный модуль, который позволяет это реализовать еще проще. Для этого в нашем модуле, в поле imports мы должны подключить еще один базовый модуль — FormsModule:

```typescript
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { FormsModule } from '@angular/forms';

import { AppComponent } from './app.component';
import { CardComponent } from './card/card.component';


@NgModule({
  declarations: [
    AppComponent,
    CardComponent
  ],
  imports: [
    BrowserModule,
    FormsModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }

```

Модуль FormsModule содержит в себе функциональность, которая позволяет работать с формами, и содержит в себе ngModel, который позволяет добавлять механизм Two Way Binding гораздо проще (мы просто оборачиваем и квадратные и в круглые скобки ngModel и в значение помещаем то поле, которое хотим связать в двустороннем порядке):

```markup
<div class="card">
  <h2>{{ title }}</h2>
  <p>{{ testText }}</p>
  
  <div>
    <!-- 2way binding через FormsModule -->
    <input type="text" [(ngModel)]="title">
    <!-- Так же, дополнительно есть событие для подписи на изменение поля -->
    <input type="text" [(ngModel)]="title" (ngModelChange)="changeHandler()">
  </div>
</div>
```

## Pass data between parent and child components

### Parent -> Child ( @Input() )

Делается это путем объявления поля через декоратор `@Input()`.

```typescript
import { Component, OnInit } from '@angular/core';
import { Input } from '@angular/core'

import { Product } from '../products';

export class ChildComponent implements OnInit {

    @Input() product!: Product;
    @Input('master') masterName = ''; // tslint:disable-line: no-input-rename
    
    // Intercept input property changes with a setter
    @Input()
    get name(): string { return this._name; }
    set name(name: string) {
        this._name = (name && name.trim()) || '<no name set>';
    } 
    private _name = '';
    
    constructor() { }
    
    ngOnInit() {
    }

}
```

Где-то в объявлении компонента:

```markup
<app-product-alerts
  [product]="some_product"
  [master]="some_master"
  [name]="some_name">
</app-product-alerts>
```

### Child -> Parent ( @Output() )

Делается это путем объявления поля через декоратор `@Output()`.

```typescript
import { Component } from '@angular/core';
import { Input } from '@angular/core';
import { Output, EventEmitter } from '@angular/core';
import { Product } from '../products';


export class ProductAlertsComponent {
  @Input() product: Product | undefined;
  @Output() notify = new EventEmitter<boolean>();
  
  notify_f(value: boolean) {
    this.notify.emit(value);
  }
}


```

Где-то в другом компоненте

```markup
<button (click)="share()">
  Share
</button>

<app-product-alerts
  [product]="product" 
  (notify)="onNotify($event)">
</app-product-alerts>
```

```typescript
export class SomeParentComponent {
    onNotify(value: boolean) {
        // some actions
    }
}
```

## Calls functions to other components

### Calls child's functions from the parent component

Делается через именование компонента — `#childComponent`. Пример:

```markup
<div class="card">
  <h2>{{ title }}</h2>
  <p>{{ testText }}</p>

  <div>
    <button (click)="changeTitle(childComponent.value)">Change Title</button>
  </div>
  
  <div>
    <!-- Другой вариант (более простой) — через локальную ссылку на элемент -->
    <input type="text" #childComponent (input)="inputHandler2(childComponent.value)" [value]="title">
  </div>
</div>
```

Другой способ (более гибкий) — инжектить дочерний компонент внутрь родительского через `@ViewChild`. Пример:

```typescript
import { AfterViewInit, ViewChild } from '@angular/core';
import { Component } from '@angular/core';
import { ChildComponent } from './child';

@Component({
    ...,
    tempate: `
        <div class="seconds">{{ seconds() }}</div>
        <app-child></app-child>
    `,
    ...
})
export class ParentComponent implements AfterViewInit {
    
    @ViewChild(ChildComponent)
    private childComponent!: ChildComponent;
    
    seconds() {return 0;}
    
    ngAfterViewInit() {
        // Redefine `seconds()` to get from the `ChildComponent.seconds` ...
        // but wait a tick first to avoid one-time devMode
        // unidirectional-data-flow-violation error
        // Кратко: это хак, тк дочерний компонент не существует, пока не отрисуется родительский,
        // а следовательно, вызов функций невозможен ( child.seconds() ).
        // Этот код дожидается 1 тик времени, прежде чем переопределить функцию
        setTimeout(() => this.seconds = () => this.childComponent.seconds, 0);
    }
    
    start() { this.childComponent.start(); }
}
```

### Using a service

Пример сервиса:

```typescript
import { Injectable } from '@angular/core';
import { Subject } from 'rxjs';

@Injectable()
export class SomeService {

  // Observable string sources
  private someStringSource = new Subject<string>();
  private someStringSource2 = new Subject<string>();

  // Observable string streams
  someStringStreams$ = this.someStringSource.asObservable()
  someStringStreams2$ = this.someStringSource2.asObservable()

  // Service message commands
  someSend(value: string) {
    this.someStringSource.next(value);
  }

  someRecv(value: string) {
    this.someStringStreams2.next(value);
  }
}
```

Родительский компонент

```typescript
import { Component } from '@angular/core';

import { SomeService } from './some.service';


@Component({
    ...,
    template: `
        <button (click)="someFunc()">Test</button>
        <app-child
          *ngFor="let value of values"
          [someInput]="value">
        </app-child>
    `,
    providers: [SomeService],
    ...
})
export class ParentComponent {
    
    history: string[] = [];    
        
    constructor(private someService: SomeService) {
        someService.someStringStreams$.subscribe(
            someValue => {
                this.history.push(`${someValue}`);
            };
        )
    }
    
    someFunc() {
        this.someService.someSend('some data');
        this.history.push('send some data');
        
    }
}
```

Дочерний компонент

```typescript
import { Component, Input, OnDestroy } from '@angular/core';

import { SomeService } from './some.service';
import { Subscription } from 'rxjs';


@Component({
    ...,
    template: `
        <button 
            (click)="confirm()">
            Test
        </button>
    `,
    ...
})
export class ChildComponent implements OnDestroy {
    @Input() someInput = '';
    somedata = '<no data>';
    subscription: Subscription;
    
    constructor(private someService: SomeService) {
        this.subscription = someService.someStringStreams2$.subscribe(
            somedata => {
                this.somedata = somedata;
            }
        )
    }
    
    confirm() {
        this.someService.someRecv(this.someInput);
    }
    
    ngOnDestroy() {
        // prevent memory leak when component destroyed
        this.subscription.unsubscribe();
    }
}
```

Отписка от событий в ngOnDestroy() крайне важна для дочерних элементов, тк это защищает от утечек памяти (для родительских компонентов этого делать не надо, тк сервис SomeService закрывается вместе с родительским компонентом).
