# Работа с итераторами

[itertools](https://docs.python.org/3/library/itertools.html) — базовая библиотека для выполнения операций над итерируемыми объектами

## Определения

### Итерируемый объект

Любой объект, имеющий методы `__iter__` или `__getitem__`, которые возвращают итераторы или могут принимать индексы. Таким образом, итерируемый объект может предоставить нам итератор (но это может не дать нам возможность выполнить функцию `next` над итерируемым объектом).

### Итератор

Объект, у которого есть метод `__next__`. Встроенная функция `iter` возвращает итератор из итерируемого объекта.

### Итерация

Процесс перебора элементов источника.

## Встроенные бесконечные последовательности

* count — последовательность чисел, в которой каждой последующее число отличается от предыдущего на фиксированную величину
* cycle — циклическая последовательность, которая строится на базе другой конечной последовательности элементов
* repeat — повторяет один и тот же элемент

```python
import itertools

itertools.count(start[, step])  # start, start + step, start + 2 * step, ...
itertools.cycle([1, 2, 3, 4])  # 1, 2, 3, 4, 1, 2, 3, 4, 1, 2, ...
itertools.repeat(1)  # 1, 1, 1, 1, ...
```

## Производные последовательности

Создаются над другими конечными последовательностями

```python
import itertools

basic1 = [1, 2, 3, 4]
basic2 = ['a', 'b', 'c']

seleciter1 = itertools.count(10, 3)
iter2 = itertools.repeat(10)
iter3 = itertools.cycle([10, 11, 12, 13])


basic1 = [1, 2, 3, 4]
basic2 = ['a', 'b', 'c']

selector = itertools.cycle([1, 0, 0, 1])


# Каждый последующий — сумма предыдущих
iter = itertools.accumulate(basic1)

# цепочка итераторов
iter = itertools.chain(basic1, basic2)
iter = itertools.chain.from_iterable([basic1, basic2])  # грубо говоря, сделать плоский список

# Выбрать элементы из последовательности
iter = itertools.compress(basic1, selector)

# Отбросить начало последовательности, пока не встретится нужный элемент (пока условие не вернет False)
# Берем последовательность, начиная с этого элемента
iter = itertools.dropwhile(lambda x: x != 3, basic1)

# Берем элементы из последовательности, пока условие будет верным 
iter = itertools.takewhile(lambda x: x != 3, basic1)

# Выбираем элементы, не подходящие под условие
iter = itertools.filterfalse(lambda x: x < 3, basic1)

# Каждый с каждым
iter = itertools.product(basic1, basic2, basic2)
iter = itertools.product(basic1, repeat=4)  # product(basic1, basic1, basic1, basic1)

# Все перестановки заданной длины
iter = itertools.permutations(basic1, r=3)  # r=None <=> r=len(basic1)

# Все комбинации из r элементов (разница от предыдущего в том, что AB=BA).
iter = itertools.combinations(basic1, r=2)

# Подпоследовательность
iter = itertools.islice(iter1, stop=4)  # Первые 4
    # islice('ABCDEFG', 2) --> A B
    # islice('ABCDEFG', 2, 4) --> C D
    # islice('ABCDEFG', 2, None) --> C D E F G
    # islice('ABCDEFG', 0, None, 2) --> A C E G

```

В документации есть и другие функции (около map, zip над итерируемыми последовательностями)
