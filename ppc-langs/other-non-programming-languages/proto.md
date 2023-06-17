# Proto

## Пример gRPC

Структура

```
/proto
    /web
        /account
            account.proto
        /admin
            admin-auth.proto
        some.proto
client.py
README.md
```

Пример какого-нибудь сервиса

```protobuf
syntax = "proto3";

package my.web.account;

import "web/admin/admin-auth.proto";

service AccountService {
  rpc Auth(AccountRequest) returns (AccountResponse) {}
}

message AccountRequest {
  string name = 1;
}

message AccountResponse {
    string token = 1;
}

```

## Stream

Одно из применений типа stream для сообщения — для отправки файлов, тк ограничение одного gRPC сообщения составляет 4194304 байт

Вот так это выглядит в `proto`-файле:

```protobuf
syntax = "proto3";

package mypackage;

service MyService {
    rpc SomeAction (stream SomeRequest) returns SomeResponse() {}
}

message SomeRequest {
    bytes file = 1;
}
message SomeResponse {
    string status = 1;
}
```

Как работать с такими данными из Python:

```python
from concurrent.futures import ThreadPoolExecutor
import logging
import threading
from typing import Iterator

import grpc

import mypackage_pb2
import mypackage_pb2_grpc

# Example async stream https://chromium.googlesource.com/external/github.com/grpc/grpc/+/master/examples/python/async_streaming/

# gRPC server
HOST = 'example.com'
PORT = 50051

# Session
kSession = '<token>'

# File = '/full/path/to/file.txt'

def request(Endpoint, Request, session=True): 
    response, call = Endpoint.with_call(
        Request,
        metadata=(
                    ("x-request-id", "2b94425f-050e-4152-88ab-2d6555439d84"),
                    ("x-session-param", kSession)
        ) if session else ()
    )

    for key, value in call.trailing_metadata():
        print('Greeter client received trailing metadata: key=%s value=%s' %
              (key, value))

    return response
    

class FileUploader:

    def __init__(self, executor: ThreadPoolExecutor, channel: grpc.Channel, filename: str) -> None:
        self._executor = executor
        self._channel = channel
        self._stub = mypackage_pb2_grpc.MyServiceStub(self._channel)
        self._filename = filename
        self._peer_responded = threading.Event()
        self._call_finished = threading.Event()
        self._consumer_future = None
        
    def _response_watcher(self, response_iterator: Iterator[mypackage_pb2.SomeRequest]) -> None:
        try:
            for response in response_iterator:
                print(response)
        except Exception as e:
            self._peer_responded.set()
            raise
            
    def upload(self) -> None:
        request = mypackage_pb2.SomeRequest()
        with open(self._filename, mode='rb') as inp_stream:
            request.file = inp_stream.read()  # b'test'

        response_iterator = self._stub.SomeAction.with_call(
            iter((request,)), 
            metadata = (
                ("x-request-id", "2b94425f-050e-4152-88ab-2d6555439d84"),
                ("x-session-param", kSession)
            )
        )

        self._consumer_future = self._executor.submit(self._response_watcher, response_iterator)
        
        
def process_call(executor: ThreadPoolExecutor, channel: grpc.Channel, filename: str) -> None:
    uploader = FileUploader(executor, channel, filename)
    uploader.upload()



def run():
    executor = ThreadPoolExecutor()
    with grpc.secure_channel(f'{HOST}:{PORT}', grpc.ssl_channel_credentials()) as channel:
        future = executor.submit(process_call, executor, channel, FILENAME)
        future.result()

if __name__ == '__main__':
    logging.basicConfig(level=logging.INFO)
    run()
```

## Android (Kotlin)

Git-repo: [https://github.com/IkeMurami/example-android-grpc-client-kotlin](https://github.com/IkeMurami/example-android-grpc-client-kotlin)

## Python

Git-repo: [https://github.com/IkeMurami/example-grpc-client-python](https://github.com/IkeMurami/example-grpc-client-python)

