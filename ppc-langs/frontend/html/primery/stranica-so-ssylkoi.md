# Страница со ссылкой

```markup
<html>
    <head>Test links</head>
    <body>
        <a href="https://example.com">Test Flight Link</a>
        <a href="https://example.com" title="some title for page">Test Flight Link</a>
    </body>
</html>
```

Любой объект можно превратить в ссылку, например, картинку:

```markup
<a href="https://example.com">
    <img src="test.png" alt="логотип">
</a>
```

Если надо создать ссылку на скачивание, а не открытие в браузере:

```markup
<a href="https://example.com/some.exe" download="some.exe">
    Нажми, чтобы скачать
</a>
```
