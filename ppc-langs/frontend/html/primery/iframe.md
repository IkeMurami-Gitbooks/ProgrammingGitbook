# iframe

## why

1. реклама
2. аналитика
3. видео с ютуба
4. игры

## iframe basic usage

Пример ниже открывает в iframe страницу приложения и ждет вызова postMessage с нее

```markup
<script>
  window.addEventListener('message', function(e) {
    fetch("/" + encodeURIComponent(e.data.data))
  }, false)
</script>
<iframe src="https://example.com"></iframe>
```

Общение происходит по [postMessage](https://developer.mozilla.org/ru/docs/Web/API/Window/postMessage)&#x20;

Родитель отправляет: `$(iframe).postMessage`&#x20;

Дочерний iframe отправляет: `window.parent.postMessage`

## iframe security

Атрибуты у iframe связанные с ИБ

* **allow** — какие API (например, Payments API) доступны сайту, подгруженному в iframe
* **referrerpolicy** — включать ли заголовок Referrer, и что туда включать
* **sandbox** — что может делать отрисованная форма (например, может ли js исполнять)

Еще пару слов про безопасность. Стоит упомянуть HTTP заголовок `X-Frame-Options`, в котором можно перечислить адреса сайтов, которые могут загружать вашу страницу в iframe:

```
X-Frame-Options: deny, sameorigin, allow-from %URI%.
```

Либо его аналог в политике CSP:

```
Content-Security-Policy: frame-ancestors source;
Content-Security-Policy: frame-ancestors source source;
```

Подробнее можно ознакомиться здесь: [habr.com/ru/post/317720](https://habr.com/ru/post/317720/)
