# Работа с классами

## Getters / setters

```python
class Student:
    def __init__(self):
        self._score = 0

    @property
    def score(self):
        return self._score

    @score.setter
    def score(self, s):
        if 0 <= s <= 100:
            self._score = s
        else:
            raise ValueError('The score must be between 0 ~ 100!')

    @score.deleter
    def score(self):
        del self._score

Yang = Student()

Yang.score=99
print(Yang.score)
# 99

Yang.score = 999
# ValueError: The score must be between 0 ~ 100!
```

## Методы класса, методы объекта, статические методы

Используем декоратор `@classmethod` для методов, которые должны быть только у классов, но не их экземпляров

```python
class Student:
    def __init__(self, first_name, last_name):
        self.first_name = first_name
        self.last_name = last_name
        self.nickname = None

    def set_nickname(self, name):
        self.nickname = name

    @classmethod # get_from_string is a class method
    def get_from_string(cls, name_string: str):
        first_name, last_name = name_string.split()
        return Student(first_name, last_name)


s = Student.get_from_string('yang    zhou')
print(s.first_name)  # yang
print(s.last_name)  # zhou

# can't call instance method directly by class name
s2 = Student.set_nickname('yang') 
# TypeError: set_nickname() missing 1 required positional argument: 'name'
```

Статические методы (объявленные как `@staticmethod`), могут быть вызваны как у объекта, так и у класса, но они не принимают ничего специфичного для класса (`cls`)

```python
class Student:
    @staticmethod
    def test(a, b):
        return a < b
```

## Конструкторы

Следует разделять `__new__` и `__init__`.

`__new__`: создает новый экземпляр (вызывается первым)

`__init__`: инициализирует новый экземпляр (вызывается вторым)

Пример построения класса-синглтона:

```python
class Singleton_Genius(object):
    __instance = None

    def __new__(cls, *args, **kwargs):
        if not Singleton_Genius.__instance:
            Singleton_Genius.__instance = object.__new__(cls)
        return Singleton_Genius.__instance

    def __init__(self, first_name, last_name):
        self.first_name = first_name
        self.last_name = last_name

s1 = Singleton_Genius("Yang", "Zhou")
s2 = Singleton_Genius("Elon", "Musk")

print(s1)
# <__main__.Singleton_Genius object at 0x7fec946b1760>
print(s2)
# <__main__.Singleton_Genius object at 0x7fec946b1760>
print(s1 == s2)
# True
print(s1.last_name)
# Musk
print(s2.last_name)
# Musk
```

## Ограничения атрибутов у класса

Чтобы нельзя было создать большое количество атрибутов, мы можем ограничить возможные атрибуты с помощью `__slots__`:

```python
class Author:
    __slots__ = ['name','hobby']
    def __init__(self,name):
        self.name = name

me = Author('Yang')

me.hobby='writing'

me.age=29
# AttributeError: 'Author' object has no attribute 'age'
```
