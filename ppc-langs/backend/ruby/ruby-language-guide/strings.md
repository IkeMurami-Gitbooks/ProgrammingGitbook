# Strings

Create string

```ruby
# Create string
myString = String.new
=> ""

myString = String.new("This is my string. Get your own string")

# В Ruby любой символ может использоваться в качестве кавычек
# Делается это через символ %
myString = %uThis is my stringu

# Documentation string
myTest = <<MYCUSTOMDOC
    My doc
    My doc2
    ...
MYCUSTOMDOC
```

Concatenate strings

```ruby
# 1
myString = "Welcome " + "to " + "Ruby!"
=> "Welcome to Ruby!"

# 2
myString = "Welcome " "to " "Ruby!"
=> "Welcome to Ruby!"

# 3
myString = "Welcome " << "to " << "Ruby!"
=> "Welcome to Ruby!"

# 4
myString = "Welcome ".concat("to ").concat("Ruby!")
=> "Welcome to Ruby!"
```

Freezing string

```ruby
myString = "Welcome " << "to " << "Ruby!"
=> "Welcome to Ruby!"

myString.freeze

myString << "hello"
TypeError: can't modify frozen string
```

Search substring

```ruby
myString = "Welcome to Ruby!"

myString["Ruby"]
=> "Ruby"

myString["Perl"]
=> nil

myString[3].chr
=> "c"

myString[11, 4]
=> "Ruby"

myString[0..6]
=> "Welcome"

myString.index("Ruby")
=> 11
```

Replace substring

```ruby
# 1

myString = "Welcome to JavaScript!"

myString["JavaScript"]= "Ruby"

puts myString
=> "Welcome to Ruby!"

# 2

myString = "Welcome to PHP Essentials!"
=> "Welcome to PHP Essentials!"

myString.gsub("PHP", "Ruby")
=> "Welcome to Ruby Essentials!"

# 3

myString = "Welcome to PHP!"
=> "Welcome to PHP!"

myString.replace "Goodbye to PHP!"
=> "Goodbye to PHP!"
```

Insert substring

```ruby
# 1

myString = "Welcome to JavaScript!"
myString[10]= "Ruby"

puts myString
=> "Welcome toRubyJavaScript!"

# 2

myString = "Welcome to JavaScript!"
=> "Welcome to JavaScript!"

myString[8..20]= "Ruby"
=> "Ruby"

puts myString
=> "Welcome Ruby!"

# 3

myString = "Paris in Spring"

myString.insert 8, " the"
=> "Paris in the Spring"
```

Split string

```ruby
myArray = "Red, Green, Blue, Indigo, Violet".split(/, /)
=> ["Red", "Green", "Blue", "Indigo", "Violet"]
```
