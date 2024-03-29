# gRPC

## Особенности

* Помимо batch-запросов (это одиночные запросы, так сделано в JSON RPC), поддерживает стриминговые запросы (канал не закрывается сетевой, запросы посылаются)
* Поддержка метаданных, таймаутов и отмены запросов на уровне протокола (в HTTP нам самим приходится придумать всякие Request ID, политики ретраев и тп). А тут передается специальным заголовком информация
* Для описания формата сообщений используется IDL. То есть мы можем описать наши сообщения как нам удобно (например, через json или proto).

## Кодогенерация

Пусть есть куча протофайлов с описанием gRPC-сервисов

```
web/
    account/
        some.proto
    admin/
        some.proto
    ...
```

Настраиваем среду (python)

```
$ python3 -m venv .venv
$ source .venv/bin/activate

$ python -m pip install grpcio
$ python -m pip install grpcio-tools
```

Генерируем python-код (pb2 и grpc код)&#x20;

```
python -m grpc_tools.protoc -I /full/path/to/proto/root/directory --python_out=. --grpc_python_out=. /full/path/to/proto/root/directory/web/*/*.proto
```

Запускаем код

```
$ python greeter_server.py
$ python greeter_client.py
```

## gRPC клиенты

Достаточно сложно тестировать gRPC API, не имея исходных proto-файлов.

На сколько этот gRPC клиент универсален? [https://github.com/ktr0731/evans](https://github.com/ktr0731/evans)

grpc\_cli — аналог curl для gRPC-сервисов

```
Список сервисов на порту:
$ grpc_cli ls localhost:8080

Подробная информация про сервис:
$ grpc_cli ls localhost:8080 helloworld.Greeter -l

Подробная информация про метод:
$ grpc_cli ls localhost:8080 helloworld.Greeter.SayHello -l

Показать типы сообщений:
$ grpc_cli type localhost:8080 helloworld.HelloRequest

Пример запроса:
$ grpc_cli call localhost:8080 SayHello "name: 'gRPC CLI'"
```

Есть интерактивный клиент `cli evans`

Аналог Postman/Insomnia — BloomRPC (но вроде Postman уже умеет в gRPC).

grpcdump — для отлова и декодирования gRPC запросов:

```
Пример подключения к сервису на порту 8080
$ grpcdump -i io -p 8080 -proto-path ./grpc/protofiles -proto-files helloworld.prot |
```
