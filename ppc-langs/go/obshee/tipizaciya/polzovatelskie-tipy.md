# Пользовательские типы

```go
type имяТипа определениеТипа  // общий вид объявления типа

// Примеры
type Stack []interface{}  // срез, содержащий любые типы
type Count int
type StringMap map[string]string
type FloatChan chan float64
type MyFunc func(rune) rune
type MyStruct struct { ... }
```
