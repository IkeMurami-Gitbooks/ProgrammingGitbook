# Асинхронная обработка сообщений / работа с очередями

## Celery Task Queue

Link: [https://docs.celeryq.dev/en/stable/getting-started/introduction.html](https://docs.celeryq.dev/en/stable/getting-started/introduction.html)

Это фреймворк, который позволяет строить обработку тасков поверх брокеров сообщений, таких как Amazon SQS, RabbitMQ, Redis и др. Как хранилище может использовать кучу всего (например, DynamoDB, MongoDB, Sqlalchemy ORMs, Redis, ...)

Статьи:

* Как запустить Celery: [https://habr.com/ru/companies/otus/articles/796413/](https://habr.com/ru/companies/otus/articles/796413/)

## kombu

Реализует несколько протоколов для брокеров сообщений, в том числе и AMQP: [https://github.com/celery/kombu](https://github.com/celery/kombu)

## pika

Реализует протокол AMQP [https://pika.readthedocs.io/en/stable/](https://pika.readthedocs.io/en/stable/)

Позволяет работать с брокерами сообщений, которые поддерживают этот протокол (например, RabbitMQ)
