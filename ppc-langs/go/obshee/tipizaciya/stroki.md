# Строки

```go
// Обозначаются либо одинарными кавычками, либо обратными апострофами
test1 := '\"test\"' // через кавычки: могут использовать экранированные последовательности, но пишутся в одну строку
test2 := `"Test 
    string"`  // через обратные апострофы: не могут исп экр последовательности, но могу писаться в несколько строк

// Конкатенация    
/// Метод 1
test3 := test1 + test2
test3 += test2

/// Метод 2: like Python
заполняем срез со строками []string
далее strings.Join()

/// Метод 3: like Java (StringBuilder), самый эффективный
var buffer bytes.Buffer
for {
    if piece, ok := someStrings(); ok {
        buffer.WriteString(piece)
    } else {
        break
    }
}
fmt.Print(buffer.String(), "\n")

// Подстроки
test3[n:m], test[:m], ..

// Размеры
len(test3)  // Количество байт
len([]rune(test3))  // Количество символов, или через utf8.RuneCountInString()

// Преобразование типов
[]rune(test1)  // string -> срез
[]byte(test1)  // string -> []byte. 
string(символы)  // []int32 или []rune -> string
string(байты)  // []byte -> string
string(int1)  // int -> string, ex: 65 -> "A"
strconv.Itoa(int1)  // int -> (string, err), ex: 65 -> "65", nil
fmt.Sprintf(x)  // x любого типа -> string, ex: 65 -> "65"
```
