# Intro

Создать проект:

```
$ django-admin startproject djangoexample
$ cd djangoexample
$ ./manage.py startapp common
```

Как выглядит приложение на Django:

```python
from django.apps import AppConfig


class MyAppConfig(AppConfig):
   def ready(self):
      # Your code
      ...
```
