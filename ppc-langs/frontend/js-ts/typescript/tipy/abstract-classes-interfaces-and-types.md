# Abstract classes, interfaces and types

## Abstract classes

Абстрактные классы представляют классы, определенные с ключевым словом abstract. Создать объект абстрактного класса напрямую нельзя. Как правило абстрактные классы описывают сущности, которые в реальности не имеют конкретного воплощения

## Interfaces

Интерфейс определяет свойства и методы, которые объект должен реализовать. Определяются ключевым свойством interface.

Интерфейсы могут быть реализованы не только объектами, но и классами. Для этого используем ключевое свойство implements. Интерфейс может содержать свойства только для чтения, значение которых нельзя менять (определяем свойства с помощью ключевого свойства readonly)

### Расширение интерфейса

TypeScript позволяет добавлять в интерфейс новые поля и методы, просто объявив интерфейс с тем же именем и определив в нем необходимые поля и методы. Например:

```typescript
interface ICar {
   color: string
}
interface ICar {
   seats: number
}
let car: ICar = {
   color: "blue",
   seats: 4,
}
```

## Отличия интерфейсов и абстрактных классов

* Интерфейсы описывают только часть функциональности объекта (определенные признаки). Абстрактный класс описывает целую категорию разных объектов, а его характеристики наследуют только те объекты, которые являются частью это категории. Например: коты и собаки — животные (абстрактный класс), а умение бегать — способность (интерфейс)
* Интерфейс описывает только поведение — методы и поля
* Наследник абстрактного класса обязан наследовать все его составляющие, а интерфейс создан только для реализации (имплементирования)

## Type

Позволяет определять псевдонимы типов с помощью ключевого слова type

```typescript
type id = number | string
```

Псевдонимы могут заимствовать и расширять код других:

```typescript
type Person = {name: string}
type Employee = Person & {company: string}
```

## Отличия Type от Interface

* interface для объектов, а type для различных типов. Если говорить про использование этих сущностей для объектов, то они взаимозаменяемы. Если говорить не об объектах, то для них мы должны использовать type:

```typescript
type Callback = (event: any) => void
type UUID = string
```

* Интерфейсы можно имплементировать, типы нельзя
* Псевдонимы типов

```typescript
type UUID = string
const uuid: UUID = '...'
```

* type не умеет в слияние в тип с одним значением, а интерфейсы умеют
* Типы могут в объединение (`|`) и в пересечение (`&`), а интерфейсы не умеют