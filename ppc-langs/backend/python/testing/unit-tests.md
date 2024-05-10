# Unit tests

## Simple unit test

```python
import pytest

def test_some():
   assert 1 == 1
```

## Async unit test

Надо поставить плагин для pytest — `pytest-asyncio`

```python
import pytest

@pytest.mark.asyncio
async def test_some():
    assert 1 == 1
```

## Test exceptions

```python
import pytest

def test_raise():
    with pytest.raises(TypeError):
        // do something that lead to the TypeError exception
        ... 
```

## Запустить тест с набором параметров

```python
@pytest.mark.parametrize('summary, owner, done',
                             [('sleep', None, False),
                              ('wake', 'brian', False),
                              ('breathe', 'BRIAN', True),
                              ('eat eggs', 'BrIaN', False),
                              ])
    def test_add_3(summary, owner, done):
        """Демонстрирует параметризацию с несколькими параметрами."""
        task = Task(summary, owner, done)
        task_id = tasks.add(task)
        t_from_db = tasks.get(task_id)
        assert equivalent(t_from_db, task)
```

## Тест асинхронного запроса с асинхронным контекстом

Инструменты:

* pytest
* pytest-mock
* pytest-asyncio
* aiohttp

Пусть есть такой код (делаем асинхронно GET http запрос к API и парсим ответ с помощью pydantic):

```python
import aiohttp
import asyncio
import pydantic


class Response(pydantic.BaseModel):
    id: int


async def fetch_data(url) -> Response:
    async with aiohttp.ClientSession() as session:
        async with session.get(url) as response:
            resp = await response.json()

            return pydantic.TypeAdapter(Response).validate_python(resp)


async def main():
    resp = await fetch_data('https://dummyjson.com/products/1')
    print('Product ID', resp.id)


if __name__ == '__main__':
    asyncio.run(main())

```

Сложность в этом кусочке:

```python
# (1) Первый раз заходим в асинхронный контекст
async with aiohttp.ClientSession() as session:
   # (2) Второй раз заходим в асинхронный контекст для вызова асинхронной функции get (3) 
   async with session.get(url) as response:
      # (4) А тут мы вызываем асинхронную функцию json
      resp = await response.json()
```

Пишем тест:

```python
import pytest
from pytest_mock import MockerFixture
from aiohttp import ClientSession, ClientResponse


from req import fetch_data  # [1]


@pytest.mark.asyncio  # [2]
async def test_exec(mocker: MockerFixture):  # [3]

    response_mock = mocker.MagicMock(spec_set=ClientResponse)  # [4]
    response_mock.__aenter__.return_value.json.return_value = {'id': 123}  # [5]

    session_mock = mocker.MagicMock(spec_set=ClientSession)
    session_mock.get.return_value = response_mock  # [6]

    # Var 1
    mocker.patch.object(ClientSession, '__aenter__', return_value=session_mock)  # [7]
    result = await fetch_data('https://dummyjson.com/products/1')  # [8]

    assert result.id == 123
```

Где:

* \[1] — импортируем нашу асинхронную функцию для тестов
* \[2] — проставляем метку для плагина pytest-asyncio, чтобы pytest смог запустить асинхронный код
* \[3] — Для удобства проставляем тип для нашей фикстуры (эту аннотацию поддерживает pytest-mock)
* \[4] — Собираем mock-объект для aiohttp.ClientResponse
* \[5] — Проставляем ответ для вызова асинхронной функции (4), при этом обрати внимание, что ответ получаем из асинхронного контекста, который формировали в (2)
* \[6] — Проставляем ответ для вызова асинхронной функции get — (3)
* \[7] — Проставляем ответ, который получаем при заходе в контекст (1)
* \[8] — Запускаем саму асинхронную функцию, для которой мы наконец-то пропатчили все сайд эффекты
