---
description: >-
  Это паттерн проектирования. По сути синтаксический сахар. Часто используется в
  Angular
---

# Декораторы

Есть 4 вида декоратора:

* Декоратор на класс
* Декоратор на свойство класса
* Декоратор на метод класса
* Декоратор на геттеры/сеттеры

В основном они работают с классами. Сами являются функциями.

Чтобы работать с декораторами, надо в `tsconfig.json` разкомментить ключ `experimentalDecorators` (это пока экспериментальная функция):

```typescript
function Log(constructor: Function) {
    // Вызывается тогда, когда создается сам класс (до создания объектов)
}

function Log2(target: any, propName: string | Symbol) {
    
}

function Log3(target: any, propName: string | Symbol, descriptor: PropertyDescriptor) {
    
}

@Log
class Component {
    @Log2
    name: string
    
    @Log3
    get componentName() {
        return this.name
    }
    
    @Log3
    logName(): void {}
}
```

Использование замыканий (функций, возвращающих другие функции) в декораторах:

```typescript
interface ComponentDecorator {
    selector: string
    template: string
}

function Component(config: ComponentDecorator) {
    return function
      <T extends {new(...args: any[]): object}>
      (Constructor: T) {
        return class extends Constructor {
            constructor(...args: any[]) {
                super(...args)
            }
        }
    }
}

@Component({
    selector: '#card',
    template: `
        <div class="card"></div>
    `
})
class CardComponent {
    
}
```
