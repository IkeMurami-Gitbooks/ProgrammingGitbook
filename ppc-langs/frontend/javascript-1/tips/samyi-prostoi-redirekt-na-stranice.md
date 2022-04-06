# Самый простой редирект на странице

```markup
<script>
  if (!document.location.hash) {
    window.location = 'https://test.example.com/blabla'
  } else {
    window.location = '/?'+document.location.hash.substr(1)
  }
</script>
```
