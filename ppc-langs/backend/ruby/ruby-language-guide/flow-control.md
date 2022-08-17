# Flow Control

## Flow Controls

### If/Else

```ruby
if customerName == "Fred"
      print "Hello Fred!"
elsif customerName == "John"
      print "Hello John!" 
elsif customername == "Robert"
      print "Hello Bob!"
end
```

### Switch

```ruby
result = case value
   when match1 then result1
   when match2 then result2
   when match3 then result3
   when match4 then result4
   when match5 then result5
   when match6 then result6
   else result7
end

#
#
#

score = 70

result = case score
   when 0..40 then "Fail"
   when 41..60 then "Pass"
   when 61..70 then "Pass with Merit"
   when 71..100 then "Pass with Distinction"
   else "Invalid Score"
end

puts result
```

## Loops

### While

```ruby

while expression do
    ... ruby code here ...
    break if i == 2
end
```

### Unless & Until

```ruby
# 1

i = 0
until i == 5
   puts i
   i += 1
end

# 2
puts i += 1 until i == 5
```

unless — это обратная история if

<pre class="language-ruby"><code class="lang-ruby"><strong>unless i >= 10
</strong>    puts "Student failed"
else
    puts "Student passed"
end</code></pre>

### For

```ruby
# 1

for i in 1..8 do
    puts i
    break if i == 2
end

# 2
for i in 1..8 do puts i end
```

### times

Это альтернатива for

```ruby
5.times { |i| puts i }

0
1
2
3
4
```
