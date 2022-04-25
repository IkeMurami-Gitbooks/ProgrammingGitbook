# HTTP запросы и обработка ответов из JS

## axios

[axios](https://github.com/axios/axios) — promise based HTTP client for the browser and node.js

Примеры см в документации или на github (там просто)

```typescript
import axios from 'axios'

const instance = axios.create({ withCredentials: true })
const response = await axios.post(`${endpointUrl}/login`, { username, password })
let data = response.data
```

## Встроенный механизм fetch

Есть такой встроенный механизм fetch - очень удобно!

```javascript
fetch("https://example.com/prepare.xsl", {method: 'GET'})
   .then(response => response.text())
   .then(text => console.log(text));
```

Ограничения:

* Нельзя делать запросы к локальным  ресурсам (если cors?) и к файлам. Надо попробовать через CORS proxy: [https://github.com/Rob--W/cors-anywhere/](https://github.com/Rob--W/cors-anywhere/). По сути: бэкенд-серверу похер на корсы, это только фронту важно (что б нельзя было подделывать запросы от имени пользователя).&#x20;

POST:

```javascript
data = {}
fetch("https://example.com/" {
    method: 'POST',
    method: 'POST', // *GET, POST, PUT, DELETE, etc.
    mode: 'cors', // no-cors, *cors, same-origin
    cache: 'no-cache', // *default, no-cache, reload, force-cache, only-if-cached
    credentials: 'same-origin', // include, *same-origin, omit
    headers: {
      'Content-Type': 'application/json'
      // 'Content-Type': 'application/x-www-form-urlencoded',
    },
    redirect: 'follow', // manual, *follow, error
    referrerPolicy: 'no-referrer', // no-referrer, *client
    body: JSON.stringify(data) // body data type must match "Content-Type" header
})
```

Оборачивание в async/await:

```javascript
async function postData(url = '', data = {}) {
    const response = await fetch(url, {...});
    return await response.json();
}

postData('https://example.com/answer', { answer: 42 })
    .then((data) => {
        console.log(data);
    });
```
