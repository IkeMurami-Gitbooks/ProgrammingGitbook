# Plugins

У pytest есть немало плагинов ([список](https://docs.pytest.org/en/latest/plugins.html)), например:&#x20;

* pytest-asyncio — плагин для тестирования асинхронных функций
* pytest-cov — оценка покрытия кода тестами
* pytest-mock — использование пакета [unittest.mock](https://docs.python.org/3/library/unittest.mock.html) с pytest

## pytest-asyncio

Добавляет маркер `pytest.mark.asyncio`, что позволяет тестировать асинхронные функции:

```python
import pytest


@pytest.fixture
def some_async_data():
   return some_async_function()


@pytest.mark.asyncio
async def test_some(some_async_data):
   obj = await some_async_data
   resp = await some_async_function()
   
   assert resp
```

## pytest-mock

использование пакета [unittest.mock](https://docs.python.org/3/library/unittest.mock.html) с pytest

Документация: [https://pytest-mock.readthedocs.io/en/latest/](https://pytest-mock.readthedocs.io/en/latest/)

```python
import pytest

# TBD
```
