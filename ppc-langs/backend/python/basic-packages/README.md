# Basic packages

## All packages

python3: [https://docs.python.org/3/library/](https://docs.python.org/3/library/)

## Notes

### contextlib

contextmanager — создание with-совместимых функций - [https://docs.python.org/3/library/contextlib.html](https://docs.python.org/3/library/contextlib.html)

### functools

`functools` - офигенная библиотека для работы с функциями

#### wraps

необходим для проброса описания хелперов и аргументов для декораторов

```python
from functools import wraps

def based_on(base_func):
    def _based_on_func(ext_func):
        @wraps(base_func)
        def _wrapper(*args, **kwargs):
            return ext_func(base_func(*args, **kwargs))
        return _wrapper
    return _based_on_func
    

def _upload(name, content, *, test=False, **options):
    ...
    
@based_on(_upload)
def upload(resp):
    ...
```

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

### operator

`operator` - базовая библиотека для переопределения базовых функций, а так же геттеров и сеттеров: [https://docs.python.org/3/library/operator.html](https://docs.python.org/3/library/operator.html)

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

1 Узнать свой IP

```python
import socket
ip_v4 = socket.gethostbyname(socket.gethostname())
```

2 Узнать, открыт ли порт:

```python
import socket
with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as sock:
  res = sock.connect_ex(('localhost', 12345))
  if res == 0:
      print("port is open")
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
$ deactivate</code></pre>

