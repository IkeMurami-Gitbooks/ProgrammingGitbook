# Работа с файловой системой

## Общее

Правильно работать с библиотекой `pathlib`, а не с `os` напрямую. Это удобно и безопасно.

## Создание директорий

```python
from pathlib import Path
Path("/my/directory").mkdir(parents=True, exist_ok=True)
```

## Write/Read file

```python
from pathlib import Path
with Path("/my/directory").open(mode="w") as out_stream:  # default: mode='r'
    out_stream.write("test")
```

## Список всех файлов в подкаталогах

```python
from pathlib import Path

path = Path('/some/path')

if path.is_dir():
    p = path.glob('**/*')
    files = [x for x in p if x.is_file()]
    for f in files:
        print(f)
```
