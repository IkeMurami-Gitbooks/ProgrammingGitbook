# Basic packages

## All packages

python3: [https://docs.python.org/3/library/](https://docs.python.org/3/library/)

## Notes

### io

Пакет io - работа со стримами. Это способ писать логи в переменные, а не в файлы или вывод.

src: [https://docs.python.org/3/library/io.html](https://docs.python.org/3/library/io.html)

Пример

```python
import io

output = io.StringIO()
output.write('First line.\n')
print('Second line.', file=output)

# Retrieve file contents -- this will be
# 'First line.\nSecond line.\n'
contents = output.getvalue()

# Close object and discard memory buffer --
# .getvalue() will now raise an exception.
output.close()
```

Именованный буфер

```python
from io import StringIO

class NamedStringIO(StringIO):
    def __init__(self, name, content):
        self.name = name
        super().__init__(content)
        
named_stream = NamedStringIO("test_stream", "init content")
```

### multiprocessing

Про multiprocessing кратко: [http://python-3.ru/page/multiprocessing](http://python-3.ru/page/multiprocessing)

Пример: по задумке — это брут чего-то

```python
import requests
from pathlib import Path

from queue import Queue, Empty as QueueEmpty
from threading import Thread
from multiprocessing.dummy import Pool as ThreadPool
from contextlib import suppress


def readFile(path: Path):
    res = []
    with path.open(mode='r') as stream:
        line = stream.readline()
        while line:
            line = line.replace('\n', '')
            res.append(line)
            line = stream.readline()

    return res


usernames = readFile(Path('/wordlists/YAWR/usernames/usernames-shortlist.txt'))
passwords = readFile(Path('/wordlists/russkiwlst/russkiwlst_top_100k.lst'))


"""
Забираем из creds креды и добавляем в очередь запросов
"""
def add2queue(*, creds_queue, tasks_queue):
    while True:
        creds = creds_queue.get()
        # print("Test")
        tasks_queue.put(creds)

"""
Делаем запрос или что угодно с нашими данными
"""
def request(creds):
    username, password = creds
    print(f'username: {username}, password: {password}')


"""
Организуем очередь
"""
def start():
    print("Start")
    creds_queue = Queue()

    """
    Забираем данные, кладем в очередь
    """
    for user in usernames:
        for pswd in passwords:
            creds_queue.put((user, pswd))

    print("Creds loaded")

    """
    Очередь тасков ограниченная
    """
    tasks_queue = Queue(maxsize=100)

    def iterate_tasks():
        with suppress(QueueEmpty):
            while True:
                yield tasks_queue.get()

    """
    Перегоняем креды в очередь на исполнение
    """
    Thread(target=lambda: add2queue(
        creds_queue=creds_queue,
        tasks_queue=tasks_queue
    )).start()

    # for task in iterate_tasks():
    #     print(task)
    pool = ThreadPool(20)  # количестов потоков
    out = pool.imap(request, iterate_tasks(), chunksize=5)

    print("Stop")


if __name__ == '__main__':
    start()


```

### pathlib: работа с файловой системой

Правильно работать с библиотекой `pathlib`, а не с `os` напрямую. Это удобно и безопасно.

#### Создание директорий

```python
from pathlib import Path
Path("/my/directory").mkdir(parents=True, exist_ok=True)
```

#### Write/Read file

```python
from pathlib import Path
with Path("/my/directory").open(mode="w") as out_stream:  # default: mode='r'
    out_stream.write("test")
```

#### Список всех файлов в подкаталогах

```python
from pathlib import Path

path = Path('/some/path')

if path.is_dir():
    p = path.glob('**/*')
    files = [x for x in p if x.is_file()]
    for f in files:
        print(f)
```

### socket

#### 1 Узнать свой IP

```python
import socket
ip_v4 = socket.gethostbyname(socket.gethostname())
```

#### 2 Узнать, открыт ли порт:

```python
import socket
with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as sock:
  res = sock.connect_ex(('localhost', 12345))
  if res == 0:
      print("port is open")
```

#### 3 Raw http request

```python
import socket

TARGET_HOST = 'www.example.com'
TARGET_PORT = 80


CONNECTION_TIMEOUT = 10
CHUNK_SIZE = 1024
HTTP_VERSION = 1.1
CRLF = '\r\n'

socket.setdefaulttimeout(CONNECTION_TIMEOUT)


def receive_all(sock, chunk_size=CHUNK_SIZE):
    """
    Gather all the data from a request.
    """
    chunks = []
    while True:
        try:
            chunk = sock.recv(int(chunk_size))
            if chunk:
                chunks.append(chunk)
            else:
                break
        except socket.timeout:
            break

    return b''.join(chunks)


# create a socket object
with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as client:
    res = client.connect_ex((TARGET_HOST, TARGET_PORT))
    client.settimeout(0.5)

    if res == 0:
        print("port is open")
        # send some data
        request = f'GET / HTTP/{HTTP_VERSION}{CRLF}' \
                + f'Host: {TARGET_HOST}{CRLF}' \
                + f'{CRLF}'
        client.sendall(request.encode())
        print(f'Send request: \n{request}')

        response = receive_all(client, CHUNK_SIZE)

        client.shutdown(socket.SHUT_RDWR)
        client.close()

        print(f'Response: {response}')
```

#### 4 Raw http request (TLS)

```python
import socket
import ssl

CRLF = '\r\n'


class RawHTTPClient:
    TARGET_HOST: str
    TARGET_PORT: int

    CONNECTION_TIMEOUT = 10
    CHUNK_SIZE = 1024
    HTTP_VERSION: str = '1.1'

    def __init__(self, host: str, port: int, http_version: str = '1.1'):
        self.TARGET_HOST = host
        self.TARGET_PORT = port
        self.HTTP_VERSION = http_version

        socket.setdefaulttimeout(self.CONNECTION_TIMEOUT)
        self.ssl_context = ssl.create_default_context()

    def request(self, raw_request: str) -> str:
        # create a socket object
        with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as http_client:
            # TLS/SSL
            with self.ssl_context.wrap_socket(http_client, server_hostname=self.TARGET_HOST) as secure_http_client:
                # connect the secure_client
                res = secure_http_client.connect_ex((self.TARGET_HOST, self.TARGET_PORT))
                secure_http_client.settimeout(0.5)

                if res == 0:
                    print("port is open")
                else:
                    return ""

                # send request
                secure_http_client.sendall(raw_request.encode())
                # receive response
                response = RawHTTPClient.receive_all(secure_http_client, self.CHUNK_SIZE)
                response = response.decode()

                # close connection
                secure_http_client.shutdown(socket.SHUT_RDWR)
                secure_http_client.close()

                # return result
                return response

    @staticmethod
    def receive_all(sock, chunk_size=CHUNK_SIZE):
        """
        Gather all the data from a request.
        """
        chunks = []
        while True:
            try:
                chunk = sock.recv(int(chunk_size))
                if chunk:
                    chunks.append(chunk)
                else:
                    break
            except socket.timeout:
                break

        return b''.join(chunks)


if __name__ == '__main__':
    client = RawHTTPClient('www.example.com', 443)
    request = [
        'GET / HTTP/1.1',
        'Host: www.example.com'
    ]
    request = CRLF.join(request) + CRLF * 2

    response = client.request(request)

    print('Response:', response)
```

### timeit

`timeit` стандартный пакет для измерений времени выполнения команд

Пример:

```python
from timeit import timeit

print(
    timeit(
        'list(zip(range(100), range(100)))', 
        number=500000
    )
)
```

### venv

Создание виртуального окружения (чтобы не мусорить в глобальные зависимости). Обязательный подход при программировании на Python.

<pre><code>// Создание виртуального окружения
$ python3 -m venv .venv

<strong>// Активация
</strong>$ source .venv/bin/activate

// Далее работаем все отсюда: устанавливаем зависимости, запускаем код

// Деактивация — выход из виртуального окружения
$ deactivate
</code></pre>

