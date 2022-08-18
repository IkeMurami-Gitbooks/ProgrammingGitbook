# Middleware: Rack

[Rack](https://github.com/rack/rack) — это прослойка между веб фреймворком (rails, sinatra, ...) и веб-сервером (puma, nginx, unicorn, ...). Это классический middleware слой, который может брать на себя классические задачи:&#x20;

* Логирование
* Управление сессией
* Профайлинг
* Кэширование
* Безопасность (отбросить запросы по IP или из-за лимитов)
* Запрос статических данных

![](<../../../../.gitbook/assets/image (3).png>)

[rack-attack](https://github.com/rack/rack-attack) — прослойка для блокирования и тротлинга

Rails on Rack: [https://guides.rubyonrails.org/rails\_on\_rack.html](https://guides.rubyonrails.org/rails\_on\_rack.html)

## Getting started

Главный конфиг файл для Rack — Rails.root/config.ru. Запустить сервер через rake:

```
$ rackup config.ru
Puma starting in single mode...
* Puma version: 5.6.4 (ruby 2.7.5-p203) ("Birdie's Version")
*  Min threads: 5
*  Max threads: 5
*  Environment: development
*          PID: 36623
* Listening on http://127.0.0.1:9292
* Listening on http://[::1]:9292
Use Ctrl-C to stop
127.0.0.1 - - [18/Aug/2022:18:16:21 +0300] "GET / HTTP/1.1" 200 400497 0.9723
127.0.0.1 - - [18/Aug/2022:18:16:21 +0300] "GET /favicon.ico HTTP/1.1" 200 - 0.1332

```

Посмотреть список объявленных middlewares:

```
$ ./bin/rails middleware
```

Посмотреть описание дефолтных middlewares — [https://guides.rubyonrails.org/rails\_on\_rack.html#internal-middleware-stack](https://guides.rubyonrails.org/rails\_on\_rack.html#internal-middleware-stack)

Включить или выключить/заменить тот или иной middleware можно через свойство `config.middleware` в скриптах-конфигах в папке `config`.

