# Заметки по языку

## Классы

```ruby
class Import
    def test # По факту test(self)
        # ...
    end
    
    def test1(data) # По факту test1(self, data)
        # ...
        if test  # По факту test(self)
        end
    end
    
import = Import.new
import.test # По факту import.test()
```

## Dynamic&#x20;

```ruby
field = :test
send("#{field}=", 123)  # По факту проинициализируется поле test = 123
```
