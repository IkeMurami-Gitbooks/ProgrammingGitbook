# Получение данных с сервера / AJAX

## AJAX

Ajax — Асинхронный JavaScript и XML.

Технология, позволяющая веб-страницам запрашивать небольшие фрагменты данных и отображать их только при необходимости, помогая решать проблему загрузки всей страницы целиком при каждом действии.

Это достигается с помощью таких API, как **XMLHttpRequest** или — более новой — **Fetch API.**

### Fetch

API-интерфейс Fetch - это, в основном, современная замена XHR - недавно он был представлен в браузерах для упрощения асинхронных HTTP-запросов в JavaScript, как для разработчиков, так и для других API, которые строятся поверх Fetch.

```javascript
fetch(url).then(function(response) {
  response.text().then(function(text) {
    poemDisplay.textContent = text;
  });
});
```

### XMLHttpRequest

`XMLHttpRequest` (который часто сокращается до XHR) является довольно старой технологией сейчас - он был изобретен Microsoft в конце 1990-х годов и уже довольно долго стандартизирован в браузерах.

```javascript
var request = new XMLHttpRequest();

request.open('GET', url);
request.responseType = 'text'; // ожидаемый ответ

/* 
    запрос — асинхронная операция. Ответ дожидаемся в onload
*/
request.onload = function() {
    poemDisplay.textContent = request.response;
};

// отправляем запрос
request.send();
```
