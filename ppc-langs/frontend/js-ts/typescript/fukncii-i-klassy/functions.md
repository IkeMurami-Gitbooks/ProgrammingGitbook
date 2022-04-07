# Functions

## Функции

```typescript
function add(a: number, b: number): number {
    return a + b
}

function test(_:number): void {}

// _ — это указывает компилятору, что параметр может не использоваться и мы это знаем

function test(): number & string {}  // это означает, что вернется объект, у которого ключи будудт иметь тип number и тип string

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
```

## Перегрузка функций

```typescript
interface MyPosition {
    x: number | undefined
    y: number | undefined
}

interface MyPositionWithDefault extends MyPosition {
    default: string
}

// Определяем возможные способы вызова
function position(): MyPosition
function position(a: number): MyPositionWithDefault
function position(a: number, b: number): MyPosition

// Определяем саму функцию
function position(a?: number, b?: number) {
    if (!a && !b) {
        return {x: undefined, y: undefined}
    }
    
    if (a && !b) {
        return {x: a, y: undefined, default: a.toString()}
    }
    
    return {x: a, y: b}
}
```

## Lambda

```typescript
const split = (a: string, s: string): string[] => a.split(s)
```
