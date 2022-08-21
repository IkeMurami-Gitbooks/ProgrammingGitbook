# Modules

Можно создавать код, который будет переиспользован в других компонентах

Они чаще складываются в lib директорию проекта (но могут и в app)

```ruby
# ./lib/my_module.rb
module MyModule
    def self.included(base)
        base.extend(ClassMethods)
    end
    
    module ClassMethods
        def another_custom
            # ...
        end
    end
    
    def custom(a)
        # do sth
    end
end
```

Далее используем этот код в нашем контроллере или где хотим:

```ruby
class MyController < ApplicationController
    include MyModule
    
    def self.func(*args)
        another_custom do |controller, value|
            controller.custom(value)
        end
    end
end
```
