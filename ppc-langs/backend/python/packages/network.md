# Network

## socket

### 1 Узнать свой IP

```python
import socket
ip_v4 = socket.gethostbyname(socket.gethostname())
```

### 2 Узнать, открыт ли порт:

```python
import socket
with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as sock:
  res = sock.connect_ex(('localhost', 12345))
  if res == 0:
      print("port is open")
```

### 3 Raw http request

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

### 4 Raw http request (TLS)

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

## aiortc

Пакет для работы с WebRTC (передача видео/аудио в режиме реального времени - стримы)

Link: [https://aiortc.readthedocs.io/en/latest/](https://aiortc.readthedocs.io/en/latest/)

## dnspython

Работа с DNS\
Примеры: [https://www.programcreek.com/python/example/82642/dns.resolver](https://www.programcreek.com/python/example/82642/dns.resolver)

```python
import dns.resolver
from dns.rdatatype import RdataType


def ip(domain: str):
    """
    Get IP-addresses
    """
    try:
        res = dns.resolver.resolve(domain, RdataType.A)
        answer = res.response.answer
    except:
        return []

    res = []
    for ans in answer:
        test = [x.address for x in ans.items.keys()]
        res.extend(test)

    return list(set(res))
    

res = ip("example.com")
```

## paramiko \[SSH]

### scp

Для работы с SSH

```
pip3 install scp
```

### paramiko

### sshtunnel

Построение туннелей через SSH

## service\_identity

Проверка сертов по DNS или ip

Link: [https://service-identity.readthedocs.io/en/stable/](https://service-identity.readthedocs.io/en/stable/)

## twisted

Событийно-ориентированный фреймворк для работы с сетью. Поддерживает множество сетевых протоколов
