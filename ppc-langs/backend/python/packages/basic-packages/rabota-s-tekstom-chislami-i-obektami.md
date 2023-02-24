# Работа с текстом, числами и объектами

## Работа с текстом

```python
import textwrap

wrapper = TextWrapper(initial_indent="* ")
# Грубо говоря будет оборачивать текст с надстройками
```

## Работа с числами

```python
import decimal
import math
```

## Работа с объектами

### copy

Позволяет [создавать ](https://docs.python.org/3/library/copy.html)копии (shallow) объектов и полные (deep) копии объектов.&#x20;

```python
import copy

copy.copy(x)
copy.deepcopy(x[, memo])
```
