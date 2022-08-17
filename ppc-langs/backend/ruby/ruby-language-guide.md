# Ruby Language Guide

## Comments

```ruby
# This is a comment line
print "Welcome to Ruby!"

=begin
Multiline
Comment
=end
```

## Constants & Variables

### Init

```ruby
# Constants & variables
a = 10
b, c = "TEST", 4

# Convert values
y = 20
=> 20
y.to_f
=> 20.0
y.to_s
=> "20"
y.to_s(2)
=> "10100"
y.to_s(16)
=> "14"
```

### Naming

```ruby
$a = 1  # A global variable
@a = 1  # An instance variable (переменная объекта)
a = 1   # A local variable
_LocalVar = 1   # A local variable too
ABC = 1  # A constant (тк любая переменная с большой буквы — константы)
LocalVar = 1 # A constant
@@a = 1  # A class variable (по сути, static поле для класса)
```

И еще есть 2 псевдо-переменные — `nil` и `self`.

Проверить область видимости переменной можно через метод `defined?`:

```ruby
x = 10
=> 10
defined? x
=> "local-variable"
```

### Global variables&#x20;

Использовать глобальные переменные **не рекомендуется**.

Однако, есть список предустановленных глобальных переменных:

| `$@`   | The location of latest error                                    |
| ------ | --------------------------------------------------------------- |
| `$_`   | The string last read by `gets`                                  |
| `$.`   | The line _number_ last read by interpreter                      |
| `$&`   | The string last matched by regexp                               |
| `$~`   | The last regexp match, as an array of subexpressions            |
| `$`_n_ | The _nth_ subexpression in the last match (same as `$~[`_n_`]`) |
| `$=`   | The case-insensitivity flag                                     |
| `$/`   | The input record separator                                      |
| `$\`   | The output record separator                                     |
| `$0`   | The name of the ruby script file currently executing            |
| `$*`   | The command line arguments used to invoke the script            |
| `$$`   | The Ruby interpreter's process ID                               |
| `$?`   | The exit status of last executed child process                  |

## Identifying a Ruby Variable Type

```ruby
# Проверить тип
y.kind_of? Integer
=> true

# Вывести класс
y.class
=> Fixnum
```

## Methods

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

## Ranges

По сути список

```ruby
# Create ranges
1..10    # Creates a range from 1 to 10 inclusive
1...10   # Creates a range from 1 to 9

# Convert to array
(1..10).to_a
=> [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

(1...10).to_a
=> [1, 2, 3, 4, 5, 6, 7, 8, 9]

('cab'..'car').to_a
=> ["cab", "cac", "cad", "cae", "caf", "cag", "cah", "cai", "caj", "cak", "cal", "cam", 
"can", "cao", "cap", "caq", "car"]
```

Методы над ranges

```ruby
words = 'cab'..'car'

words.min         # get lowest value in range
=> "cab"

words.max         # get highest value in range
=> "car"

words.include?('can') # check to see if a value exists in the range
=> true

words.reject {|subrange| subrange < 'cal'} # reject values below a specified range value
=> ["cal", "cam", "can", "cao", "cap", "caq", "car"]

words.each {|word| puts "Hello " + word} # iterate through each value and perform a task

(1..20) === 15        # 15 лежит в диапазоне [1, 20] ?
=> true
```

Ranges in case Statements

```ruby
score = 70

result = case score
   when 0..40: "Fail"
   when 41..60: "Pass"
   when 61..70: "Pass with Merit"
   when 71..100: "Pass with Distinction"
   else "Invalid Score"
end

puts result
```

## Arrays

```ruby
# Create
arr = Array.new
arr.empty?
=> true
arr.size
=> 0

arr = Array.new(7)
=> [nil, nil, nil, nil, nil, nil, nil]

arr = Array.new(3, "test")
=> ["test", "test", "test"]

arr = Array[ "1", "2", "3" ]
=> ["1", "2", "3"]

arr[0]  # or arr.at(0)
=> "1"

# arr[-1]
# arr.first, arr.last

arr = Array[ "1", 456, "test" ]

# Поиск элементов
arr.index(456)
=> 1

# Подмассивы и срезы
arr[2, 6]
arr[2..6]
arr.slice(2..6) 
```

Другие операции над массивами

```ruby
arr1 = [1, 2, 3]
arr2 = [4, 5, 6]

# Concatinate
arr1 + arr2
=> [1, 2, 3, 4, 5, 6]
# Or arr1.concat(arr2)

# Append to existing array
arr1 << 7
=> [4, 5, 6, 7]

# Differenct
arr1 - arr2

# Intersection
arr1 & arr2

# Union (duplicates are removed)
arr1 | arr2

# Unique array elements
arr1.uniq

# Если мы хотим, чтобы все повторения удалились из оригинального массива, используем метод uniq!
arr1.uniq!

# Push & Pop
arr1.push 4
=> [1, 2, 3, 4]
arr1.pop
=> 4

# Insert
arr1.insert(1, 8)
=> [1, 8, 2, 3]

# Change value
arr1[1] = 5
arr1[1..2] = 5, 6

# Delete elem
arr1.delete_at(1)  # Delete by index
arr1.delete(1)  # Delete by value

# Sort
arr1.sort
arr1.reverse
arr1.sort!  # Save result

```

Array compression

```
==
<=>
.eql?

The == method returns true if two arrays contain the same number of elements and the same contents for each corresponding element.
The eql? method is similar to the == method with the exception that the values in corresponding elements are of the same value type.
Finally, the <=> method (also known as the "spaceship" method) compares two arrays and returns 0 if the arrays are equal, -1 one if the elements are less than those in the other array, and 1 if they are greater.
```

