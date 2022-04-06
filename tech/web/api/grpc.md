# gRPC

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
