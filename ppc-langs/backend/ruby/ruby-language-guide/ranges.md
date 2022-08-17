# Ranges

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
