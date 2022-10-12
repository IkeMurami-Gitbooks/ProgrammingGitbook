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

## iframe security

Атрибуты у iframe связанные с ИБ

* allow
* sandbox

Еще пару слов про безопасность. Стоит упомянуть HTTP заголовок X-Frame-Options, в котором можно перечислить адреса сайтов, которые могут загружать вашу страницу в iframe:

```
X-Frame-Options: deny, sameorigin, allow-from %URI%.
```

Либо его аналог в политике CSP:

```
Content-Security-Policy: frame-ancestors source;
Content-Security-Policy: frame-ancestors source source;
```

Подробнее можно ознакомиться здесь: [habr.com/ru/post/317720](https://habr.com/ru/post/317720/)
