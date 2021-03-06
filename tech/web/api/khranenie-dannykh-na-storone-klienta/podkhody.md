# Подходы

## Старый подход: куки

Куча проблем и ограничений

## Новый подход: Web Storage и IndexedDB

Современные браузеры имеют гораздо более простые и эффективные API для хранения данных на стороне клиента, чем при использовании файлов cookie.

* The [Web Storage API](https://developer.mozilla.org/en-US/docs/Web/API/Web\_Storage\_API) обеспечивает очень простой синтаксис для хранения и извлечения данных, состоящих из пар 'ключ' : 'значение'. Это полезно, когда вам просто нужно сохранить некоторые простые данные, такие как имя пользователя, вошли ли они в систему, какой цвет использовать для фона экрана и т. д.
* The [IndexedDB API](https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB\_API) обеспечивает браузер полной базой данных для хранения сложных данных. Это может быть использовано для хранения полных наборов записей клиентов и даже до сложных типов данных, таких как аудио или видео файлы.

## Что нас ждет в будущем: Cache API

Некоторые современные браузеры поддерживают новое [`Cache`](https://developer.mozilla.org/ru/docs/Web/API/Cache) API. Этот API предназначен для хранения HTTP-ответов на конкретные запросы и очень полезен для таких вещей, как хранение ресурсов сайта в автономном режиме, чтобы впоследствии сайт можно было использовать без сетевого подключения. Cache обычно используется в сочетании с [Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service\_Worker\_API), однако это не обязательно.

Использование Cache и Service Workers - сложная тема, и мы не будем подробно останавливаться на ней в этой статье, хотя приведем простой пример [Offline asset storage](https://developer.mozilla.org/ru/docs/Learn/JavaScript/Client-side\_web\_APIs/Client-side\_storage#offline\_asset\_storage) в разделе ниже.
