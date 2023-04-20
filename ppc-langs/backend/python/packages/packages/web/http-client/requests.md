# requests

обертка для http-запросов

## Cases

### File Upload (multipart)

```python
import requests

zipfile = open('my.zip', 'rb')

# multipart request
r = requests.post(
    self.BASE_URL + "/themes",
    cookies={f"test_{some_cookies_key}": self.SESSION},
    headers={
        "User-Agent": USER_AGENT,
        "X-CSRF-Token": token,
        "X-Requested-With": "XMLHttpRequest",
    },
    files={
        "file_name": zipfile,
    },
    data={
        "theme_id": theme_id,
        "token": token,
    },
)
```

### Set Proxy

Начиная с последних версий либ requests/urllib3

```
r = requests.post(
    ...
    proxies = {
        'https': 'http://127.0.0.1:8080'
    }
    ...
)
```

В старых версиях либ:

```python
r = requests.post(
    ...
    proxies = {
        'http': 'http://127.0.0.1:8080',
        'https': 'https://127.0.0.1:8080'
    }
    ...
)
```
