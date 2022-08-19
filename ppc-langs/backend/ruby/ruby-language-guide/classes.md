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

## Переопределяем конструктор по умолчанию

```ruby
class Class             # Дефолтный встроенный класс Class
    alias old_new new   # переопределяем дефолтный конструктор
    def new(*args)
        print "Creating a new ", self.name, "\n"
        old_new(*args)
    end
end

=begin
    Именованный класс
=end

class Name
    # Some code ...
end

n = Name.new
```

## Анонимные классы (неименованные)

```ruby
=begin
    Анонимный класс
=end

fred = Class.new do
    def method
        "hello"
    end
end

a = fred.new
puts a.method
```

## Public, private and protected methods

Можно через self определять приватные методы

```ruby
class PrivateMethod

    def pub
        "Public"
    end

    def self.priv
        "Private"
    end

end


p = PrivateMethod.new
puts p.pub
puts p.priv

=begin

    $ ruby classes.rb
    Public
    Traceback (most recent call last):
    classes.rb:16:in `<main>': undefined method `priv' for #<PrivateMethod:0x00007f9fc301d5c8> (NoMethodError)

=end
```

А можно через public/private директивы

```ruby
class PrivateMethod

    public

    def pub
        "Public"
    end
    
    protected
    
    def protected_method
        "Protected"
    end

    private

    def priv
        "Private"
    end
    
    def priv2
        "Another private method"
    end

end


p = PrivateMethod.new
puts p.pub
puts p.priv

=begin

    $ ruby classes.rb
    Public
    Traceback (most recent call last):
    classes.rb:16:in `<main>': undefined method `priv' for #<PrivateMethod:0x00007f9fc301d5c8> (NoMethodError)

=end
```
