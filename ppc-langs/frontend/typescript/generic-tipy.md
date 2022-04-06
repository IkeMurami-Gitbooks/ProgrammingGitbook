# Generic-типы

## Базовый пример

```typescript
const arrayOfNumbers: Array<number> = [1, 2, 3, 4]

function reverse<T>(array: T[]): T[] {
    return array.reverse()
}
```

## Generic и Promise

Использование generic в промисах:

```typescript
// Через Generic тип указываем с каким типом работает Promise
const promise = new Promise<string>(resolve => {
    setTimeout(() => {
        resolve('Promise resolved')
    }, 2000)
})

// data имеет тип string теперь
promise.then(data => {
    console.log(data)
})

/*
Что здесь происходит:
1. создается callback-функция: data => {console.log(data)}
2. resolve инициализируется значением callback-функции через вызов у promise метода then
3. Когда promise запущен, внутри него будет вызван resolve
*/
```

## Generic типы для создания более гибких функций и проверки самого себя на этапе разработки

Использование для получения доступа к информации о полях

```typescript
function mergeObjects<T, R>(a: T, b: R): T & R {
    return Object.assign({}, a, b)
}

const merged = mergeObjects({name: 'Test'}, {age: 20})

console.log(merged.name)  // Без указания типа через Generic, мы бы не могли получить доступ к полям созданного объекта


// ========

/*
Здесь есть проблема: Object.assign ожидает объекты на вход, а не простые типы
Сейчас мы можем передать и простой тип: mergeObjects('abc', 'def')
И на выходе гавно получим

Вот так можно ввести ограничение (constraints) на тип в generic:
*/
function test<T extends object>(a: T) {
    // ...
}


// =====
// Другой пример
// Описание возвращаемого значения:
interface ILength {
    length: number
}

function withCount<T extends ILength>(value: T): {value: T, count: string} {
    return {
        value,
        count: `Count: ${value.length}`
    }
}



// ========
// Еще пример встроенной проверки: 
//    первый тип — объект
//    второй тип — указываем, что это тип ключа первого объекта! (через keyof оператор)
function getObjectValue<T extends object, R extends keyof T>(obj: T, key: R) {
    return obj[key]
}

const person = {
    test: 123
}
console.log(getObjectValue(person, 'test'))
console.log(getObjectValue(person, 'notexist')) // Компилятор укажет ошибку
```

## Generic типы в классах

```typescript
class Collection<T extends number | script | boolean> {
    
    constructor(private _items: T[] = []) {}
    
    add(item: T) {
        this._items.push(item)
    }
    
    remove(item: T) {
        this._items = this._items.filter(i => i !== item)
    }
    
    get items(): T[] {
        return this._items
    }
}

const strings = new Collection(['a', 'b', 'c'])
strings.add('d')
strings.remove('a')
```

## Partial: Generic типы и временные объявления

```typescript
interface Car {
    model: string
}

/*
  Здесь будет ошибка: car должно иметь поле model, а оно инициализируется пустым объектом
*/
function test(model: string): Car {
    const car: Car = {}
    
    if (model.length > 0) {
        car.model = model
    }
    
    return car
}

/*
  Поправим это с помощью отложенной инициализации
*/
function test2(model: string): Car {
    // Мы говорим компилятору: "Да, мы знаем, что у объекта нет поля model, 
    // оно будет проинициализировано позднее"
    
    const car: Partial<Car> = {}  
    
    if (model.length > 0) {
        car.model = model
    }
    
    // А здесь делаем привидение типа от Partial<Car> к Car
    return car as Car  
}
```

## Readonly инструмент

Этот инструмент позволяет блокировать изменение значений полей

```typescript
const cars: Readonly<Array<string>> = ['Ford', 'Audi']
cars.shift()  // Здесь компилятор выдаст ошибку: изменять значения внутри нельзя!
```

То же самое можно делать и с объектами.
