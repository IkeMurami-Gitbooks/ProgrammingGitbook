# Classes

## Объявление

```typescript
class Typescript {
    version: string
    
    constructor(version: string) {
        this.version = version
    }
    
    info(name: string) {
        return `[${name}]: Typescript version is ${this.version}`
    }
}

class Car {
    readonly model: string
    readonly numberOfWheels: number = 4
    
    constructor(theModel: string) {
        this.model = theModel
    }
}

// Запись идентична предыдущей
class Car {
    readonly numberOfWheels: number = 4
    constructor(readonly model: string) {}
}
```

## Модификаторы (protected, private, public)

```typescript
class Animal {
    protected voice: string = ''    // protected — доступен в классе Animal и во всех функциях классов, которые будут наследоваться от класса Animal
    public color: string = 'black'  // public — модификатор по умолчанию
    
    private go() {
        console.log('Go')
    }
}
```

## Абстрактые классы и методы

Они ни во что не компилируются, но нужны на этапе разработки

```typescript
abstract class Component {
    abstract render(): void
    abstract info(): string
}

class AppComponent extends Component {
    
    render(): void {
        console.log('Component is renderer')
    }

    info(): string {
        return 'some info'
    }
}
```
