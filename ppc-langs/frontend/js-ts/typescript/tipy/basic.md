# Базовые типы и создание своих типов

## Создание переменной

```typescript
let changeble
const notChangeble: number = 1
const notChangeble2: string[] = ['a', 'b', 'c', 'd']

notChangeble2 = ['test'] // Нельзя, но
notChangeble2.shift()    // можно

```

## Объявление типа

```typescript
// Простые
const isTest: boolean = false
const int: number = 1

const count: number = 44
const someFloat: number = 4.2
const someNum: number = 3e10

const message: string = 'Some String'

// Массивы
const numberArray: number[] = [1, 1, 2, 3, 4]
const numberArray2: Array<number> = [1, 1, 2, 3]  // Через Generic-запись

// Tuple — массив из разных типов данных
const contact = ['Test', 123]
const contact2: [string, number] = ['Test', 123]

// Any — любой тип, чтоб можно было переопределять тип переменной
let testAny: any = 42
testAny = 'Test'

// Возвращаем ничего
function sayMyName(name: string): void {
    console.log(name)
}
sayMyName('Test')

// Never — это тип, который указывает, что функция может не выполниться до конца 
// или никогда не завершится
function throwError(message: string): never {
    throw new Error(message)
}

function infinite(message: string): never {
    while(true) {}
}
```

## Создание своих типов

```typescript
// тип как элиас
type Login = string
const login: Login = 'admin'

// элиас для двух типов
type ID = string | number
const id1: ID = 1234
const id2: ID = '1234'

// тип null и undefined
type SomeType = string | null | undefined

// Тип-перечисление
type SomeTypeEnum = 'value1' | 'value2' | 'value3' 
const test: SomeTypeEnum = 'value2' // correct
const test2: SomeTypeEnum = 'value4'  // incorrect

// составной тип
type User = {
    _id: number
    name: string
    email: string
    createdAt: Date
}
```
