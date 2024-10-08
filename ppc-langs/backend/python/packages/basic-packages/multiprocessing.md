# Работа с многопоточностью

## Как выбрать количество пулов

CPU-based и I/O-based операции: [https://stackoverflow.com/a/41780357](https://stackoverflow.com/a/41780357)

Если работаем с сетью, то можно выбрать побольше: 20, 40, 100

Если с файловой системой, то так не сработает, мы только забьем ресурсы и будет еще медленнее работать. Оптимально выбрать количество ядер помноженное на 2.

## multithreading

Многопоточное выполнение над локальными процессами (например, над файловой системой)

Пример:

```python
import os
import itertools
# Глубоко под копотом Pool из threading то же самое, что и Pool из multiprocessing.pool
from multiprocessing.dummy import Pool as ThreadPool

pool = ThreadPool(os.cpu_count() * 2)
results_iterator = pool.imap_unordered(do_something, some_iterator)
pool.close()
```

## multiprocessing

Про multiprocessing кратко: [http://python-3.ru/page/multiprocessing](http://python-3.ru/page/multiprocessing)

Логика, стоящая за выбором chunksize: [https://stackoverflow.com/questions/53751050/multiprocessing-understanding-logic-behind-chunksize](https://stackoverflow.com/questions/53751050/multiprocessing-understanding-logic-behind-chunksize)

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

## Timer

Выполняем задачу периодически, пока не вернется нужный результат:

```python
from typing import Generic, TypeVar, Callable
from threading import Timer


T = TypeVar('T')


class TimerWithResult(Timer, Generic[T]):
    result: T

    def __init__(self, interval: int, function: Callable[..., T], args=[], kwargs={}):
        self._original_function = function
        super(TimerWithResult, self).__init__(
            interval, self._do_execute, args, kwargs)

    def _do_execute(self, *a, **kw):
        self.result = self._original_function(*a, **kw)

    def join(self) -> T:
        super(TimerWithResult, self).join()
        return self.result


def do_something(a: int, b: str, c: bool = True) -> str:
    print('Try get result')
    return f'Result: {a} & {b} & {c}'


def is_success(result: T) -> bool:
    return False


def wait_result(interval: int, function: Callable[..., T], args=[], kwargs={}) -> T:
    result = function(*args, **kwargs)

    while not is_success(result):
        timer = TimerWithResult[T](interval, function, args=args, kwargs=kwargs)
        timer.start()
        result = timer.join()

    return result


if __name__ == '__main__':
    result = wait_result(2, do_something, [1, 'abc'], {'c': False})
    print('TIMER', result)

```
