---
description: Ни во что не компилируются, нужны только на этапе разработки
---

# Создание сложных типов: Interfaces

## Создание интерфейсов и приведение типов

```typescript
// Объявление
interface Rect {
    readonly id: string  // ReadOnly Field
    color?: string   // Необязатеельное поле
    size: {
        width: number
        height: number
    }
}

// Создание объекта
const rect1: Rect = {
    id: 'test',
    size: {
        width: 1
        height: 2
    }
}


const rect2: Rect = {
    id: 'test1',
    size: {
        width: 1
        height: 2
    },
    color: '#ccc'
}

/*
* Хоть это и const, но поля мы можем менять, так все компилится в js, а js это позволяет
**/
rect2.color = 'black'  

// Приведение типов
const rect3 = {} as Rect
const rect4 = <Rect>{}  // Старая запись
```

## Наследование интерфейсов

```typescript
interface RectWithArea extends Rect {
    getArea: () => number
}

const rect5: RectWithArea = {
    id: '123',
    size: {
        width: 1
        height: 2
    },
    getArea(): number {
        return this.size.width * this.size.height
    }
}
```

## Классы и интерфейсы

```typescript
interface IClock {
    time: Date
    setTime(date: Date): void
}

class Clock implements IClock {
    time: Date = new Date()
    
    setTime(date: Date): void {
        this.time = date
    }
}
```

## Интерфейсы с большим количеством ключей

Допустим, мы хотим описать объект с большим количеством полей, и к тому же мы еще не знаем какие поля будут. Для этого есть специальный синтаксис и ключевое слово `key`:

```typescript
interface Styles {
    [key string]: string
}

const css: Styles = {
    border: '1px solid black',
    marginTop: '2px',
    borderRadius: '5px'
}
```
