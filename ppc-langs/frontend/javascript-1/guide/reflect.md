# Reflect

## About

[`Reflect`](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global\_Objects/Reflect) это встроенный объект, предоставляющий методы для перехватываемых операций JavaScript. Это те же самые методы, что имеются в [обработчиках Proxy](https://developer.mozilla.org/ru/docs/conflicting/Web/JavaScript/Reference/Global\_Objects/Proxy/Proxy). Объект `Reflect` не является функцией.

`Reflect` помогает при пересылке стандартных операций из обработчика к целевому объекту.

Например, метод `Reflect.has()` это тот же [`оператор in`](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Operators/in) но в виде функции:

```
Reflect.has(Object, 'assign'); // true
```

## Улучшенная функция `apply`

В ES5 обычно используется метод [`Function.prototype.apply()`](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global\_Objects/Function/apply) для вызова функции в определенном контексте (с определенным `this)` и с параметрами, заданными в виде массива (или [массиво-подобного объекта](https://developer.mozilla.org/ru/docs/Web/JavaScript/Guide/Indexed\_collections#working\_with\_array-like\_objects)).

```
Function.prototype.apply.call(Math.floor, undefined, [1.75]);
```

С методом [`Reflect.apply`](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global\_Objects/Reflect/apply) эта операция менее громоздка и более понятна:

```
Reflect.apply(Math.floor, undefined, [1.75]);
// 1;

Reflect.apply(String.fromCharCode, undefined, [104, 101, 108, 108, 111]);
// "hello"

Reflect.apply(RegExp.prototype.exec, /ab/, ['confabulation']).index;
// 4

Reflect.apply(''.charAt, 'ponies', [3]);
// "i"
```

## Проверка успешности определения нового свойства

Метод [`Object.defineProperty`](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global\_Objects/Object/defineProperty), в случае успеха операции, возвращает объект, а при неудаче вызывает ошибку [`TypeError`](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global\_Objects/TypeError). Из-за этого определение свойств требует обработки блоком [`try...catch`](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Statements/try...catch) для перехвата возможных ошибок. Метод [`Reflect.defineProperty`](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global\_Objects/Reflect/defineProperty), в свою очередь, возвращает успешность операции в виде булева значения, благодаря чему возможно использование простого [`if...else`](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Statements/if...else) условия:

```
if (Reflect.defineProperty(target, property, attributes)) {
  // успех
} else {
  // что-то пошло не так
}
```
