# Pytest CLI

## Install

```
pip install pytest pytest-asyncio
pytest --help
```

## Usage

Ниже приведен краткий обзор соглашений об именах, чтобы ваш тестовый код можно было обнаружить с помощью pytest:

* Тестовые файлы должны быть названы `test_<something>.py` или `<something>_test.py`.
* Методы и функции тестирования должны быть названы `test_<something>`.
* Тестовые классы должны быть названы `Test<Something>`.

```
pytest       # search into a current directory all functions started from 'test_' or ended with '_test'
pytest path/to/test_1.py path/to/test_2.py 
pytest path/to/test_1.py::test_constructor       # Run the 'test_asdict' function only

```

Запустить только один тест:

```
pytest path/to/test_1.py::test_function
pytest path/to/test_1.py::TestClass::test_method
```

Вывести информацию о тестах

```
-v — подробно
-q — кратко
```

Другие ключи посмотреть можно в статье на habr или `pytest --help`.

## Пример структуры файлов для тестов

```
/tests
   /func            — Functional tests
   /unit            — Unit-tests
   pytest.ini       — (optional) настройки pytest
   conftest.py      — Hook functions and fixtures
```

_Hook functions_ являются способом вставки кода в часть процесса выполнения pytest для изменения работы pytest. _Fixtures_ — функции, которые будут вызваны до и после выполнения теста (например, для подключения к ресурсу и отключению от него).
