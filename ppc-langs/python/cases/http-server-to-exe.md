# HTTP Server to exe

замечание: pyinstaller - не кросс-платформенный: виндовый бинарь на линухах не соберешь

## Python 3

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

## Python 2

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
