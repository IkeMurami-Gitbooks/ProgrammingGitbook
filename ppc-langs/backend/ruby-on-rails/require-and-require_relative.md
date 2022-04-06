# require and require\_relative

```ruby
require 'full/path/script'
# Этот путь ищется по путям, обозначенным в переменной $LOAD_PATH (глобальная в ruby)
# Например: 
# LOAD_PATH = /test/ruby
# Искать будет в /test/ruby/full/path/script.rb
```

```ruby
require_relative 'relative/path/script'
# Ищет в текущей директории относительно скрипта
#
#
```
