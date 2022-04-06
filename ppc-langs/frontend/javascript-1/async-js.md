# Async JS

## Главное

Как это использовать в старых браузерах: [https://babeljs.io/](https://babeljs.io)

## Синхронный JS

```javascript
function hello() { return "Hello" };
hello();
```

## Асинхронный JS

### Async/Await

#### Async

```javascript
// ex1
async function hello() { return "Hello" };
hello();

// ex2
let hello = async function() { return "Hello" }; // возвращает promise
hello();

// ex3 — то же, что и ex2, но через стрелочные функции
let hello = async () => { return "Hello" };
hello();

// ex1, ex2, ex3 делают одно и то же

// Чтобы получить значение, которое возвращает Promise, 
// мы как обычно можем использовать метод .then():
hello().then((value) => console.log(value))
// или еще короче
hello().then(console.log)

```

Итак, ключевое слово `async`, превращает обычную функцию в асинхронную и результат вызова функции оборачивает в Promise. Также асинхронная функция позволяет использовать в своем теле ключевое слово await, о котором далее.

#### Await

Асинхронные функции становятся по настоящему мощными, когда вы используете ключевое слово [await](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/await)  — по факту, **`await` работает только в асинхронных функциях**. Мы можем использовать await перед promise-based функцией, чтобы остановить поток выполнения и дождаться результата ее выполнения (результат Promise). В то же время, остальной код нашего приложения не блокируется и продолжает работать.

Вы можете использовать `await` перед любой функцией, что возвращает Promise, включая Browser API функции.

Небольшой пример:

```javascript
async function hello() {
  return greeting = await Promise.resolve("Hello");
};

hello().then(alert);
```

### Promises

[`Promise`](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global\_Objects/Promise) (промис, англ. "обещание") - это объект, представляющий результат успешного или неудачного завершения асинхронной операции. Так как большинство людей пользуются уже созданными промисами, это руководство начнем с объяснения использования вернувшихся промисов до объяснения принципов создания.&#x20;

В сущности, промис - это возвращаемый объект, в который вы записываете два коллбэка вместо того, чтобы передать их функции.

```javascript
function doSomething() {
  return new Promise((resolve, reject) => {
    console.log("Готово.");
    // Успех в половине случаев.
    if (Math.random() > .5) {
      resolve("Успех")
    } else {
      reject("Ошибка")
    }
  })
}

doSomething().then(successCallback, failureCallback);
```

Обработка ошибок Promise:

```javascript
function errorHandler(someError) {}

doSomething()
    .then(successCallback, failureCallback)
    .catch(errorHandler);

или

doSomething()
    .then(successCallback, failureCallback)
    .catch((error) => {...});
```

### Примеры

#### Переписываем Promises с ипользованием async/await

Давайте посмотрим на пример из предыдущей статьи:

```javascript
fetch('coffee.jpg')
.then(response => {
  if (!response.ok) {
    throw new Error(`HTTP error! status: ${response.status}`);
  } else {
    return response.blob();
  }
})
.then(myBlob => {
  let objectURL = URL.createObjectURL(myBlob);
  let image = document.createElement('img');
  image.src = objectURL;
  document.body.appendChild(image);
})
.catch(e => {
  console.log('There has been a problem with your fetch operation: ' + e.message);
});
```

Давайте перепишем код используя async/await и оценим разницу.

```javascript
async function myFetch() {
  let response = await fetch('coffee.jpg');

  if (!response.ok) {
    throw new Error(`HTTP error! status: ${response.status}`);
  } else {
    let myBlob = await response.blob();

    let objectURL = URL.createObjectURL(myBlob);
    let image = document.createElement('img');
    image.src = objectURL;
    document.body.appendChild(image);
  }
}

myFetch()
.catch(e => {
  console.log('There has been a problem with your fetch operation: ' + e.message);
});
```

Согласитесь, что код стал короче и понятнее — больше никаких блоков `.then()` по всему скрипту!

Так как ключевое слово `async` заставляет функцию вернуть Promise, мы можем использовать гибридный подход:

```javascript
async function myFetch() {
  let response = await fetch('coffee.jpg');
  if (!response.ok) {
    throw new Error(`HTTP error! status: ${response.status}`);
  } else {
    return await response.blob();
  }
}

myFetch().then((blob) => {
  let objectURL = URL.createObjectURL(blob);
  let image = document.createElement('img');
  image.src = objectURL;
  document.body.appendChild(image);
}).catch(e => console.log(e));
```

#### Await и Promise.all()

```javascript
async function fetchAndDecode(url, type) {
  let response = await fetch(url);

  let content;

  if (!response.ok) {
    throw new Error(`HTTP error! status: ${response.status}`);
  } else {
    if(type === 'blob') {
      content = await response.blob();
    } else if(type === 'text') {
      content = await response.text();
    }

    return content;
  }

}

async function displayContent() {
  let coffee = fetchAndDecode('coffee.jpg', 'blob');
  let tea = fetchAndDecode('tea.jpg', 'blob');
  let description = fetchAndDecode('description.txt', 'text');

  let values = await Promise.all([coffee, tea, description]);

  let objectURL1 = URL.createObjectURL(values[0]);
  let objectURL2 = URL.createObjectURL(values[1]);
  let descText = values[2];

  let image1 = document.createElement('img');
  let image2 = document.createElement('img');
  image1.src = objectURL1;
  image2.src = objectURL2;
  document.body.appendChild(image1);
  document.body.appendChild(image2);

  let para = document.createElement('p');
  para.textContent = descText;
  document.body.appendChild(para);
}

displayContent()
.catch((e) =>
  console.log(e)
);
```

Фишка в вызове Promise.all — дожидаемся выполнения всего кода
