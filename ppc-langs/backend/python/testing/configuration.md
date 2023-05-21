# Configuration

* _pytest.ini_: Это основной файл конфигурации Pytest, который позволяет вам изменить поведение по умолчанию. Поскольку вы можете внести довольно много изменений в конфигурацию, большая часть этой главы посвящена настройкам, которые вы можете сделать в `pytest.ini`.
* _conftest.py_: Это локальный плагин, позволяющий подключать хук-функции и фикстуры для каталога, в котором существует файл `conftest.py`, и всех его подкаталогов.
* _`__init__.py`_: При помещении в каждый test-подкаталог этот файл позволяет вам иметь идентичные имена test-файлов в нескольких каталогах test.

Вы можете получить список всех допустимых параметров для `pytest.ini` из `pytest --help`:

```
$ pytest --help
...
[pytest] ini-options in the first pytest.ini|tox.ini|setup.cfg file found:                                                                 

  markers (linelist)       markers for test functions                                                                                      
  empty_parameter_set_mark (string) default marker for empty parametersets                                                                 
  norecursedirs (args)     directory patterns to avoid for recursion                                                                       
  testpaths (args)         directories to search for tests when no files or directories are given in the command line.                     
  console_output_style (string) console output: classic or with additional progress information (classic|progress).                        
  usefixtures (args)       list of default fixtures to be used with this project   
  ...
```

Подробнее: [https://habr.com/ru/articles/448796/](https://habr.com/ru/articles/448796/)
