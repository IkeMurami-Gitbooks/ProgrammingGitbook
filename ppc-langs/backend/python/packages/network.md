# Network

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
