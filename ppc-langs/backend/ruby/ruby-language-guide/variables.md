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

<table data-header-hidden><thead><tr><th width="163.5">Variable Name</th><th>Variable Value</th></tr></thead><tbody><tr><td><code>$@</code></td><td>The location of latest error</td></tr><tr><td><code>$_</code></td><td>The string last read by <code>gets</code></td></tr><tr><td><code>$.</code></td><td>The line <em>number</em> last read by interpreter</td></tr><tr><td><code>$&#x26;</code></td><td>The string last matched by regexp</td></tr><tr><td><code>$~</code></td><td>The last regexp match, as an array of subexpressions</td></tr><tr><td><code>$</code><em>n</em></td><td>The <em>nth</em> subexpression in the last match (same as <code>$~[</code><em>n</em><code>]</code>)</td></tr><tr><td><code>$=</code></td><td>The case-insensitivity flag</td></tr><tr><td><code>$/</code></td><td>The input record separator</td></tr><tr><td><code>$\</code></td><td>The output record separator</td></tr><tr><td><code>$0</code></td><td>The name of the ruby script file currently executing</td></tr><tr><td><code>$*</code></td><td>The command line arguments used to invoke the script</td></tr><tr><td><code>$$</code></td><td>The Ruby interpreter's process ID</td></tr><tr><td><code>$?</code></td><td>The exit status of last executed child process</td></tr></tbody></table>

### Identifying a Ruby Variable Type

```ruby
# Проверить тип
y.kind_of? Integer
=> true

# Вывести класс
y.class
=> Fixnum
```
