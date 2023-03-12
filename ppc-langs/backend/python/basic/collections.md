# Collections

В Python есть 4 встроенные коллекции — [list](https://docs.python.org/3/library/stdtypes.html#list), [dict](https://docs.python.org/3/library/stdtypes.html#dict), [set ](https://docs.python.org/3/library/stdtypes.html#set)и [tuple](https://docs.python.org/3/library/stdtypes.html#tuple).

## Базовые коллекции

Над базовыми коллекциями поддерживаются куча интересных функций над базовыми операторами.

### set и frozenset

Элементы множества должны быть [hashable](https://docs.python.org/3/glossary.html#term-hashable).

```python
set1 = set(iterable)
frozenset1 = frozenset(iterable)

# Еще Set можно создать следующим образом:
set2 = {1, 2}
set3 = {c for c in 'abracadabra' if c not in 'abc'}
```

#### Create Object for Set

`__eq__` — нужно переопределять для сравнения (это происходит при добавлении в множество)

`__str__` — нужно переопределять, что бы одинаковые объекты выглядели одинаково

`__hash__` — для добавления в множество

```python
class Record(BaseModel):
    address: Address
    length: str
    record_type: str
    string: str

    def __eq__(self, other):
        if self.string == other.string:
            return True
        else:
            return False

    def __str__(self):
        return self.string

    def __hash__(self):
        return hash(str(self))
```

### Tuple

Это неизменяемый кортеж значений. Можно создать одним из следующих способов

```python
tuple1 = ()
tuple2 = a,
tuple3 = (a,)
tuple4 = a, b, c
tuple5 = (a, b, c)
tuple6 = tuple()
tuple7 = tuple(iterable)
```

Реализует базовые функции над последовательностями. Обращение к элементу кортежа по индексу. Через [collections.namedtuple()](https://docs.python.org/3/library/collections.html#collections.namedtuple) (см ниже) можно сделать именованный кортеж; в этом случае можно будет обращаться по имени.&#x20;

## Специализированные коллекции

Базовый пакет [collections ](https://docs.python.org/3/library/collections.html)реализует несколько дополнительных коллекций
