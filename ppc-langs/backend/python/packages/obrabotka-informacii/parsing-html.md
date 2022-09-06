# Parsing HTML

## About

`Beautiful Soup 4` [https://pypi.org/project/beautifulsoup4/](https://pypi.org/project/beautifulsoup4/) - сбор информации с web страниц

## Примеры на bs4

```python
import bs4
import requests

r = self.get("https://example.com/blabla")
if r.status_code == 200:
    parsed_r = bs4.BeautifulSoup(resp.content, features="html.parser")
    # find <input name="test" />
    test = parsed_r.select_one("input[name='test']")
    
```



