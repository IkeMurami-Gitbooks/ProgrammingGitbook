# Network & Protocols

`dnspython` - Работа с DNS\
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

`service_identity` - [https://service-identity.readthedocs.io/en/stable/](https://service-identity.readthedocs.io/en/stable/)\
Проверка сертов по DNS или ip

`twisted` - событийно-ориентированный фреймворк для работы с сетью. Поддерживает множество сетевых протоколов

SSH: \
`scp` - pip3 install scp - для работы с SSH\
`paramiko` - работа с SSH\
`sshtunnel` - построение туннелей через SSH

`aiortc` [https://aiortc.readthedocs.io/en/latest/](https://aiortc.readthedocs.io/en/latest/) - Пакет для работы с webrtc (передача видео/аудио в режиме реального времени - стримы)

`requests` - обертка для http-запросов

`zeep` - [https://docs.python-zeep.org/en/master/](https://docs.python-zeep.org/en/master/) - SOAP-клиент. Читает wsdl-файл и можем работать с SOAP-сервером.



