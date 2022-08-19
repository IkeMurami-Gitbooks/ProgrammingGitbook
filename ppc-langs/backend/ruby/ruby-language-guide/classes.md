# Classes

## Basic

Пример класса

```ruby
class BankAccount
   def initialize ()
   end

   def test_method
        puts "The class is working"
   end
end

account = BankAccount.new()
account.test_method   # Exec method
=> The class is working
```

## Accessors (getters and setters)

Use method-accessors

Getters

```ruby
class BankAccount
   def myAccessor
      @myAccessor = "test"
   end

   def initialize ()
   end

   def test_method
        puts "The class is working"
        puts myAccessor
   end
end

account = BankAccount.new()
account.myAccessor    # Init accessor
account.test_method   # Exec method
=> The class is working
=> test
```

Setters

```ruby
class BankAccount
    # Getter
    def myValue
        @value
    end
    
    # Setter
    def myValue=( val )
        @value = val
    end
    
    def initialize ()
    end
end

acc = BankAccount.new()
acc.myValue = "test"
puts acc.myValue
```

## Inheritance

```ruby
class ChildClass < ParentClass
end
```
