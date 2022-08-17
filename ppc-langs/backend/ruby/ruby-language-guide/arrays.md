# Arrays

<pre class="language-ruby"><code class="lang-ruby"><strong># Create
</strong>arr = Array.new
arr.empty?
=> true
arr.size
=> 0

arr = Array.new(7)
=> [nil, nil, nil, nil, nil, nil, nil]

arr = Array.new(3, "test")
=> ["test", "test", "test"]

arr = Array[ "1", "2", "3" ]
=> ["1", "2", "3"]

arr[0]  # or arr.at(0)
=> "1"

# arr[-1]
# arr.first, arr.last

arr = Array[ "1", 456, "test" ]

# Поиск элементов
arr.index(456)
=> 1

# Подмассивы и срезы
arr[2, 6]
arr[2..6]
arr.slice(2..6) </code></pre>

Другие операции над массивами

```ruby
arr1 = [1, 2, 3]
arr2 = [4, 5, 6]

# Concatinate
arr1 + arr2
=> [1, 2, 3, 4, 5, 6]
# Or arr1.concat(arr2)

# Append to existing array
arr1 << 7
=> [4, 5, 6, 7]

# Differenct
arr1 - arr2

# Intersection
arr1 & arr2

# Union (duplicates are removed)
arr1 | arr2

# Unique array elements
arr1.uniq

# Если мы хотим, чтобы все повторения удалились из оригинального массива, используем метод uniq!
arr1.uniq!

# Push & Pop
arr1.push 4
=> [1, 2, 3, 4]
arr1.pop
=> 4

# Insert
arr1.insert(1, 8)
=> [1, 8, 2, 3]

# Change value
arr1[1] = 5
arr1[1..2] = 5, 6

# Delete elem
arr1.delete_at(1)  # Delete by index
arr1.delete(1)  # Delete by value

# Sort
arr1.sort
arr1.reverse
arr1.sort!  # Save result

```

Array compression

```
==
<=>
.eql?

The == method returns true if two arrays contain the same number of elements and the same contents for each corresponding element.
The eql? method is similar to the == method with the exception that the values in corresponding elements are of the same value type.
Finally, the <=> method (also known as the "spaceship" method) compares two arrays and returns 0 if the arrays are equal, -1 one if the elements are less than those in the other array, and 1 if they are greater.
```

