# Beatifiers

## JSON

\
`1. JSON`\
`echo '{"test":"a"}' | python3 -m json.tool`\
`или`\
`python3 -m json.tool in_json.txt out_json.txt`

## JS

```
pip install jsbeautifier
js-beautify -h
js-beautify -o out.js in.js

или код:
import jsbeautifier
data = 'import blabla;'
data = jsbeautifier.beautify(data)
```

## XML/HTML

```python
from bs4 import BeautifulSoup

bs = BeautifulSoup(data, 'xml')  # 'html.parser'
data = bs.prettify()
```
