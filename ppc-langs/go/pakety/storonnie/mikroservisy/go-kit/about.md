# About

go-kit - библиотека для создания микросервисной архитектуры (в Readme есть ссылки на проекты, которые его используют) [https://github.com/go-kit/kit](https://github.com/go-kit/kit)

## Go kit architecture

Три главных уровня в архитектуре приложения разработанных с помощью Go kit это:

* транспортный уровень: определяет протокол общения — HTTP, gRPC, Thrift, AMQP, ..
* уровень эндпоинтов: описывает запрот-ответ в стиле RPC. Каждый запрос, связанный с бизнес-логикой из сервиса попадает на соотв обработчик на уровне Endpoint.
* уровень сервиса: бизнес-логика, определяется как интерфейс и соотв реализация.

## Использует кодогенерацию

There are several third-party tools that can generate Go kit code based on different starting assumptions.

* [devimteam/microgen](https://github.com/devimteam/microgen)
* [GrantZheng/kit](https://github.com/GrantZheng/kit)
* [kujtimiihoxha/kit](https://github.com/kujtimiihoxha/kit) (unmaintained)
* [nytimes/marvin](https://github.com/nytimes/marvin)
* [sagikazarmark/mga](https://github.com/sagikazarmark/mga)
* [sagikazarmark/protoc-gen-kit](https://github.com/sagikazarmark/protoc-gen-kit)
* [tuneinc/truss](https://github.com/tuneinc/truss)

## Связан с др проектами (см github)



