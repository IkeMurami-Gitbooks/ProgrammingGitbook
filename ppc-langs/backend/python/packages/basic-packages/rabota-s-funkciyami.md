# Работа с функциями

В Python есть офигенная базовая библиотека для прокачки функций — [functools](https://docs.python.org/3/library/functools.html). Эта библиотека для функций, которые воздействуют на другие функции или возвращают функции.

## cache

Кэширует результат вызова функции. Если функция уже вызывалась, то нам сразу вернется ответ:

```python
from functools import cache

@cache
def add(a: int, b: int) -> int:
    return a + b
    
add(1, 2)  # Здесь функция вычисляется
add(1, 2)  # Сейчас возвращается предыдущий ответ
```

## wraps

необходим для проброса описания хелперов и аргументов для декораторов

```python
from functools import wraps

def based_on(base_func):
    def _based_on_func(ext_func):
        @wraps(base_func)
        def _wrapper(*args, **kwargs):
            return ext_func(base_func(*args, **kwargs))
        return _wrapper
    return _based_on_func
    

def _upload(name, content, *, test=False, **options):
    ...
    
@based_on(_upload)
def upload(resp):
    ...
```
