# Работа с файлами и IO

## pathlib

Работа с файловой системой. Правильно работать с библиотекой `pathlib`, а не с `os` напрямую. Это удобно и безопасно.

### Создание директорий

```python
from pathlib import Path
Path("/my/directory").mkdir(parents=True, exist_ok=True)
```

### Write/Read file

```python
from pathlib import Path
with Path("/my/directory").open(mode="w") as out_stream:  # default: mode='r'
    out_stream.write("test")
```

### Работа с временными файлами и временной директорией

```python
import tempfile
from pathlib import Path

with tempfile.TemporaryDirectory() as tmpdir:
    path = Path(tmpdir, 'test.txt')
    # ...
```

### Список всех файлов в подкаталогах

```python
from pathlib import Path

path = Path('/some/path')

if path.is_dir():
    p = path.glob('**/*')
    files = [x for x in p if x.is_file()]
    for f in files:
        print(f)
```

## io

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

