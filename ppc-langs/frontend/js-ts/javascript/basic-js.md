# Basic JS

### null and undefined

`undefined` — переменной не было присвоено значение

`null` — намеренно сделали переменную пустой

### use strict

директива, которая заставляет ваш код исполняться в строгом режиме

### Desctructuring (деструктуризация объекта)

Это способ извлечения значений из объектов и массивов:

```javascript
// array
let [,b] = "a b".split(" ")
// object
let t = {a: "", b: 1, c: true}
let {a: somea, b: someb, c} = t
console.log(somea, someb, c)

let {c, ...props} = t
```

### this

This указывает на объект, который выполняет текущий кусок JS-кода

