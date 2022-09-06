# io

Пакет io - работа со стримами. Это способ писать логи в переменные, а не в файлы или вывод.

src: [https://docs.python.org/3/library/io.html](https://docs.python.org/3/library/io.html)

Пример

```python
import io

output = io.StringIO()
output.write('First line.\n')
print('Second line.', file=output)

# Retrieve file contents -- this will be
# 'First line.\nSecond line.\n'
contents = output.getvalue()

# Close object and discard memory buffer --
# .getvalue() will now raise an exception.
output.close()
```

Именованный буфер

```python
from io import StringIO

class NamedStringIO(StringIO):
    def __init__(self, name, content):
        self.name = name
        super().__init__(content)
        
named_stream = NamedStringIO("test_stream", "init content")
```
