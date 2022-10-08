# Разное

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
