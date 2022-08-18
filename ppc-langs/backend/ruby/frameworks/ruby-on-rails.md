# Web: Ruby on Rails

Свое простое приложение на RoR: [https://guides.rubyonrails.org/getting\_started.html](https://guides.rubyonrails.org/getting\_started.html)

Так же, тут можно найти структуру типичного приложения на rails.

По умолчанию используется http сервер puma.

## Rails Routes

Описаны в `/config/route.rb` и скриптах `/config/routes/...` .

Про DSL Rails Routes: [https://guides.rubyonrails.org/routing.html](https://guides.rubyonrails.org/routing.html)

Этот механизм перенаправляет url на контроллеры или Rack приложению. Позволяет не хардкодить в куче мест строки.

Если все зависимости у вас локально подтянуты, то можно посмотреть все роуты командой :

```
./bin/rails routes
```
