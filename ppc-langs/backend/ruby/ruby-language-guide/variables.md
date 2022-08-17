# Variables

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

### Identifying a Ruby Variable Type

```ruby
# Проверить тип
y.kind_of? Integer
=> true

# Вывести класс
y.class
=> Fixnum
```
