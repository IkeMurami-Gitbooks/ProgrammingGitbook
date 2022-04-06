---
description: Способ коммуницировать в обход CORS?
---

# postMessage

Пример

Отправка сообщения (postMessage(data: Object, allow-origin: string))

```markup
<script>
    parent.postMessage({type: 'onload', data: window.location.href}, '*')
</script>
```

Приемник сообщения (message принимает все сообщения, так вроде делать не надо, если не собираешься принимать сообщения от других веб-приложений):

```markup
<script>
  window.addEventListener('message', function(e) {
    fetch("/" + encodeURIComponent(e.data.data))
  }, false)
</script>
```
