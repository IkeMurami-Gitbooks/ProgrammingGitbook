# Web Storage API

Все данные вашего веб-хранилища содержатся в двух объектоподобных структурах внутри браузера: [`sessionStorage`](https://developer.mozilla.org/ru/docs/Web/API/Window/sessionStorage) и [`localStorage`](https://developer.mozilla.org/ru/docs/Web/API/Window/localStorage). Первый сохраняет данные до тех пор, пока браузер открыт (данные теряются при закрытии браузера), а второй сохраняет данные даже после того, как браузер закрыт, а затем снова открыт. Мы будем использовать второй в этой статье, так как он, как правило, более полезен.

```javascript
// Set
localStorage.setItem('name','Chris');
// Get
var myName = localStorage.getItem('name');
myName
// Remove
localStorage.removeItem('name');
var myName = localStorage.getItem('name');
myName
```

Одной из ключевых особенностей веб-хранилища является то, что данные сохраняются между загрузками страниц (и даже в случае закрытия браузера, в случае `localStorage`).

Существуют **отдельные хранилища** данных для **каждого** домена (каждый отдельный веб-адрес загружается в браузер). Вы увидите, что если вы загрузите два веб-сайта (например, google.com и amazon.com) и попытаетесь сохранить элемент на одном веб-сайте, он не будет доступен для другого веб-сайта.
