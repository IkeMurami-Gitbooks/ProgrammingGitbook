# Работа с контекстом

Суть контекста — выполнение действий при входе и выходе из контекста, а так же передача параметров между компонентами, имеющими один контекст.

## contextlib

Создание **with**-совместимых функций — [https://docs.python.org/3/library/contextlib.html](https://docs.python.org/3/library/contextlib.html)

### Создание своего ContextManager

Определяет два абстрактных класса — `AbstractContextManager` и `AbstractAsyncContextManager`. Они обязывают реализовать функции `object.__enter__()` и `object.__exit__()` (или их асинхронные аналоги `object.__aenter__()` и `object.__aexit__()`).

```python
import asyncio
import contextlib
import contextvars


class MyAsyncContextManager(contextlib.AbstractAsyncContextManager):
    async def __aenter__(self):
        print('entering context')
        return await super().__aenter__()

    async def __aexit__(self, exc_type, exc, tb) -> bool | None:
        print('exiting context')
        return await super().__aexit__(exc_type, exc, tb)
        

async def main():
    async with MyAsyncContextManager() as test:
        print(f'Hello {test}')


loop = asyncio.run(main())

"""
entering context
Hello <__main__.MyAsyncContextManager object at 0x000001FAC3E945E0>
exiting context
"""
```

### Использование встроенных декораторов

```python
import asyncio
import contextlib
import contextvars


@contextlib.asynccontextmanager
async def some_func(*args, **kwds):
    resource = "test"
    try:
        print(f"before yield: {args}, {kwds}")
        yield resource
        print("after yield")
    except:
        print("exception")
    finally:
        print("release something")


async def main():
    async with some_func(456, test=123) as test:
        print(f'Into with: {test}')


asyncio.run(main())

"""
before yield: (456,), {'test': 123}
Into with: test
after yield
release something
"""
```

## contextvars

Link: [https://docs.python.org/3/library/contextvars.html](https://docs.python.org/3/library/contextvars.html)

```python
from contextvars import ContextVar

var: ContextVar[int] = contextvars.ContextVar('var', default=42)

value = var.get()
var.set(1234)
```
