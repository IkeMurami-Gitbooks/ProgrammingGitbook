# Operators



```python
!s (apply str()) 
and 
!r (apply repr()) 
can be used to convert the value before it is formatted.

Пример:
print(f'Test {s!r}')
```

Можно переопределить работу операторов над вашими объектами, определив функции из пакета [operator](https://docs.python.org/3/library/operator.html). А так же, с помощью этого пакета можно переопределять геттеры и сеттеры.
