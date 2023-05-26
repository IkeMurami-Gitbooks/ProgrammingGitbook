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

### dataclasses

Этот пакет реализует необходимые методы для работы с объектами, например, их сравнение:

```python
from dataclasses import dataclass

class AnotherPoint:
    x: int
    y: int

A = Point(2, 3)
B = Point(2, 3)
print(A == B)
# False

@dataclass
class Point:
    x: int
    y: int
    
A = Point(2, 3)
B = Point(2, 3)
print(A == B)
# True
```

### copy

Позволяет [создавать ](https://docs.python.org/3/library/copy.html)копии (shallow) объектов и полные (deep) копии объектов.&#x20;

```python
import copy

copy.copy(x)
copy.deepcopy(x[, memo])
```
