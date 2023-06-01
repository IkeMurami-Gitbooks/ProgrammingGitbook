# Работа с другими сервисами

## adb

ADB Python [https://github.com/google/python-adb](https://github.com/google/python-adb)

```
brew install libusb
brew install openssl
pip3 install adb
```

## AWS

boto3

## Docker

Работа с docker контейнерами (и с Docker Hub)\
pip3 install docker\
Doc: [https://docker-py.readthedocs.io/en/latest/containers.html](https://docker-py.readthedocs.io/en/latest/containers.html)\
[https://docs.docker.com/develop/sdk/examples/](https://docs.docker.com/develop/sdk/examples/)

## Git

Git in Python: [https://www.pygit2.org/](https://www.pygit2.org/)

## Google Services

Для работы с Google сервисами есть API Manager. С помощью него можно получить доступ, например, к Google Sheets

## Mitmproxy

Link: [https://github.com/mitmproxy/mitmproxy](https://github.com/mitmproxy/mitmproxy)

## MongoDB

```python
# pip install pymongo
import pymongo

SERVER = 'server.localhost'
PORT = '27017'
DATABASE = 'database_name'
USER = 'database_user'
PASSWORD = 'database_password'

client = pymongo.MongoClient(f"mongodb://{USER}:{PASSWORD}@{SERVER}:{PORT}")
mydb = client[DATABASE]

# Надо проверить будет этот код на чем-то реальном
try: 
    # don't require auth
    res = client.admin.command('ping')
    print('PING', res)
    # require password
    res = mydb.command("serverStatus")
    print('STATUS', res)
except Exception as e: print(e)
else: print("You are connected!")
client.close()
hon
```

## nmap

* `python-libnmap` — обёртка над `nmap`, на сколько хороша - хз
* `python-nmap` — ее вроде много кто использует
