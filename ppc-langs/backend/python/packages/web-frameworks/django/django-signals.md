# Django Signals

[Сигналы](https://docs.djangoproject.com/en/5.0/topics/signals/) — очень похожая реализация паттерна Pub-Sub в Django для общения между компонентами одного приложения.

В Django есть встроенные сигналы, на которые можно подписываться (например, изменение каких-нибудь настроек), а можно создавать свои сигналы

Пример встроенного сигнала:

```python
from django.core.signals import setting_changed
```

Пример создания своего сигнала:

```python
from django.dispatch import Signal

my_signal = Signal()
```

Подписаться на сигнал (стать receiver'ом в терминах Django):

```python
from django.core.signals import setting_changed
def my_callback(sender, **kwargs):
    print("Request finished!")

setting_changed.connect(my_callback)
```

или через декоратор receiver():

```python
from django.core.signals import setting_changed
from django.dispatch import receiver


@receiver(setting_changed)
def my_callback(sender, **kwargs):
    print("Setting changed!")
```

Отправить сигнал:

```python
my_signal.send(sender=self.__class__, arg1=1, arg2="test", ...)
```

## Полный пример

Например, мы хотим подписаться на событие сохранения нашей модели Django:

```python
from django.db.models.signals import post_init, post_save
from django.dispatch import receiver

from my_model import MyModel


@receiver(post_save, sender=MyModel)
def my_receiver(sender, instance, created, **kwargs):
   # Если кто-то будет сохранять модель MyModel, то этот обработчик вызовется после обновления
   # проверяем поля instance (это будет типа MyModel), решаем что делать
   ...
```
