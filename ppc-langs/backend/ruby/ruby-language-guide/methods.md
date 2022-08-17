# Methods

### Methods

```ruby
// General
def name( arg1, arg2, arg3, ... )
   .. ruby code ..
   return value
end

// Позиционные аргументы
def name( *args )
   args.each { |string| puts string }
end

name("first")
name("first", "second")
```

В Ruby функции могут возвращать только одно значение или объект. Надо вернуть несколько значений? — Используйте массивы.

### Aliases

Можно сделать ссылку на метод:

```ruby
def name(a)
    ...
end

alias my_alias name

my_alias(123)
```
