# Время выполнения операции

### timeit

`timeit` стандартный пакет для измерений времени выполнения команд

Пример:

```python
from timeit import timeit

print(
    timeit(
        'list(zip(range(100), range(100)))', 
        number=500000
    )
)
```
