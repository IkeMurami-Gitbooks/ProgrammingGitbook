# Разные соглашения

Синтаксически символ доллара (`$`) не имеет особого значения в [идентификаторах JavaScript](https://www.ecma-international.org/ecma-262/7.0/index.html#sec-names-and-keywords).

Однако иногда используется условное обозначение, чтобы указать, что переменная содержит [`Observable`](http://reactivex.io/rxjs/class/es6/Observable.js\~Observable.html) или что функция вернет символ `Observable`.

```typescript
const some$
function test$() {}
```
