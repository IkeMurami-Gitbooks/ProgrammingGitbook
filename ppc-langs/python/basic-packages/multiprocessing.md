# multiprocessing

Про multiprocessing кратко: [http://python-3.ru/page/multiprocessing](http://python-3.ru/page/multiprocessing)

## Пример

По задумке — это брут чего-то

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


usernames = readFile(Path('/Users/user/Work/tools/pentest/wordlists/YAWR/usernames/usernames-shortlist.txt'))
passwords = readFile(Path('/Users/user/Work/tools/pentest/wordlists/russkiwlst/russkiwlst_top_100k.lst'))


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
