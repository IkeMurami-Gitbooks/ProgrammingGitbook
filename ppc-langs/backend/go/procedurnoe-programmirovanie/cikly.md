# Циклы

Идентичные формы записи циклов (но первая короче)

```go
import log

// foreach
for row := range someArray {
    if row[0] <= 0 {
        // ...
    }
    else {
        log.Fatal("invalid ...")
    }
}

// foreach string
for index, char := range someString {  // type for char - rune: Go automate decode as UTF-8

}

// classic for
for row := 0; row < len(someArray); row++ {
    // ...
}

// infinite loop
for {
    break
}

for element := range someChannnel {
    
}


// init & post statement - are optional
for ; sum < 1000; {
		sum += sum
}

// like while
for sum < 1000; {
		sum += sum
}
```

## Метки

Это способ выходить из глубоких циклов. Метки поддерживаются инструкциями break и continue.

Пример

```go
found := false
FOUND:
for row := range table {
    for column := range table[row] {
        if someExpr {
            found = true
            break FOUND
        }
    }
}
```
