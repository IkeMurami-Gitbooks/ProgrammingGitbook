# aiohttp

Пример асинхронного клиента с поддержкой контекста

```python
import aiohttp
import logging
import typing
import urllib.parse


class HTTPClient:
    USER_AGENT = 'My user agent'
    base_url: str

    def __init__(self, base_url: str, logger: logging.Logger) -> None:
        self.base_url = base_url
        self.logger = logger

    async def __aenter__(self):
        await self.connect()
        return self
    
    async def __aexit__(self, exc_type, exc_val, exc_tb):
        await self._session.close()

    async def connect(self):
        connector = aiohttp.TCPConnector(force_close=True)
        self._session = aiohttp.ClientSession(
            base_url=self.base_url,
            connector=connector,
            headers={
                'User-Agent': self.USER_AGENT
            }
        )

    @staticmethod
    def urlencode(data: typing.Dict) -> str:
        return urllib.parse.urlencode(data)

    async def get_info(self, id: str, search: str) -> typing.List[str]:
        query = {
            'search': search
        }
        async with self._session.get(f'/api/{id}?{HTTPClient.urlencode(query)}') as response:
            if response.ok:
                data = await response.json()

                return data
            else:
                print(response)
                return list()
```

Пример GraphQL клиента:

```python
import aiohttp
import json

class GQLClient:
    USER_AGENT = 'My user agent'
    base_url: str

    def __init__(self, base_url: str):
        self.base_url = base_url


    async def __aenter__(self):
        await self.connect()
        return self

    async def __aexit__(self, exc_type, exc_val, exc_tb):
        await self._session.close()

    async def connect(self):
        headers = {
            'User-Agent': self.USER_AGENT,
            "Content-Type": "application/json",
        }
        self._session = aiohttp.ClientSession(base_url=self.base_url, headers=headers)

    async def query_url(self, query, **kwargs):
        async with self._session.post(
            "/api", data=json.dumps({"query": query}), **kwargs
        ) as resp:
            return json.loads(await resp.read())["data"]
        
    

```

Пример использования подобного клиента

```python
async with HTTPClient("https://example.com", None) as http_client:
    data = await http_client.get_info("1124", "search smth")
```
