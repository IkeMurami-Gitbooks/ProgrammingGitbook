# Санитайзеры

## Markupsafe

[markupsafe](https://pypi.org/project/MarkupSafe/) — эскейпинг символов для использования строк в html и xml документах. Реализует интерфейс `__html__`, и может принимать объекты от других библиотек, которые реализуют этот же интерфейс

Пример:

```python
from markupsafe import Markup

Markup.escape('<a href=x>t</a>')
```

Однако, надо иметь ввиду, что следующая запись наоборот позволяет прокинуть html без анкодинга:

```python
from markupsafe import Markup

Markup('<a href=x>t</a>')
```

## bleach

[bleach](https://pypi.org/project/bleach/) — библиотека от Mozilla для экранирования html конструкций

Пример:

```python
import bleach

ALLOWED_TAGS = [
    'br', 'h1'
]
ALLOWED_ATTRS = {
    '*': ['class'],
    'a': ['href'],
    'img': ['src'],
    'div': ['id', 'some']
}

cleaned = bleach.clean(html_text, tags=ALLOWED_TAGS, attributes=ALLOWED_ATTRS)
```
