# Составные типы

Обозначаются struct

```go
type test struct {
    a float64
    b MySomeInterface
}
```

Можно объявлять одинаковые переменные вместе

```go
// Одно и то же:
type test struct {
    a float64
    b float64
}



type test struct {
    a, b float64
}
```
