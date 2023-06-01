# Python script -> binary file

## nuitka

Пару раз бывало что нужно закинуть питонячую тулзу на похеканую машинку, но нет возможности ставить пакеты. Можно упаковать venv и перенести, а можно скопмилировать бинарь:

```
$ python3 -m nuitka --standalone --onefile dnsrecon.py
```

Tips: перед сборкой гляньте версию `libc` на целевой системе, собирайте на близкой версии.

## pyinstaller

Note: pyinstaller — не кросс-платформенный: виндовый бинарь на линухах не соберешь

### Пример: Python HTTP server -> exe

#### Python 3

```python
import http.server
import socketserver

PORT = 80

Handler = http.server.SimpleHTTPRequestHandler

with socketserver.TCPServer(("", PORT), Handler) as httpd:
    print("serving at port", PORT)
    httpd.serve_forever()


"""
Компиляция:

pip3 install pyinstaller
pyinstaller web.py --onefile
"""
```

#### Python 2

```python
import SimpleHTTPServer
import SocketServer

PORT = 80

Handler = SimpleHTTPServer.SimpleHTTPRequestHandler

httpd = SocketServer.TCPServer(("", PORT), Handler)

print "serving at port", PORT
httpd.serve_forever()

"""
Компиляция:

pip install pyinstaller
pyinstaller web.py --onefile
"""
```
