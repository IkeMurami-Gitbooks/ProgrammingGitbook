# Libraries



## Templates

### Twig

Один из популярных шаблонизаторов в PHP.

Кратко:

```
{{ value }} - вставить переменную или результат выражения
{# value #} - комментарий в шаблоне, не попадает в итоговый html-документ
{% raw %}
{% value %}
{% endraw %} - условный блок-конструкция или цикл
```

Что значит знак минус? [Это значит](https://stackoverflow.com/questions/16634412/minus-in-twig-block-definition) убрать пробельные символы слева или справа:

```
{% raw %}
{% set value = 'test' %}
{% endraw %}
<li>  {{- value }}   <il>
render -> <li>test   <il>
```

Чтобы избежать нарушения контекста страницы, можно (и нужно!) использовать [https://twig.symfony.com/doc/3.x/tags/autoescape.html](https://twig.symfony.com/doc/3.x/tags/autoescape.html) автоматическое кодирование вставляемых данных.&#x20;

При этом можно менять поведение для конкретных строк:

```
{{ value }} - по умолчанию кодирует как html
{{ value|raw }} — не кодировать
{{ value|escape }} — кодировать со стратегией по умолчанию (html)
{{ value|e('js') }}
```

Стратегии: html, js, css, url, html\_attr, не кодировать (raw, false),&#x20;

Функции, возвращающие template's, по умолчанию возвращают safe markup. Например: macro и parent ([https://twig.symfony.com/doc/3.x/tags/macro.html](https://twig.symfony.com/doc/3.x/tags/macro.html) и [https://twig.symfony.com/doc/3.x/functions/parent.html](https://twig.symfony.com/doc/3.x/functions/parent.html)).
