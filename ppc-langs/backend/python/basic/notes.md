# Заметки

То, что не должно меняться, лучше оборачивать в `tuple`

Поднять локальный сервер: `python -m http.server [port] --directory /tmp/`

`getattr(obj, 'fieldName')` - получить поле по названию

`timeit` стандартный пакет для измерений времени выполнения команд\
Пример:\
`from timeit import timeit`\
`print(timeit('list(zip(range(100), range(100)))', number=500000))`

Установить пакет конкретной версии:\
1\. Удалить пакет, если уже установлен\
2\. pip install -I (i большое) packet==version

Проблема при чтении html по кодировке utf-8: Попробовать считать кодировкой "ISO-8859-1"

Создание установочного пакета: [http://klen.github.io/create-python-packages.html](http://klen.github.io/create-python-packages.html)

1 Свой IP

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

`print(blabla, file=file, flush=True)` flush - не будет буферизовать данные: без этого принт ждёт добива блока до 4кб например

Про наследование, mro и super - [https://habr.com/ru/post/62203](https://habr.com/ru/post/62203)
