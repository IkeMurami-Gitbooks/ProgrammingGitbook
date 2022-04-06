# Перечисления

```go
type Weakday int
const (
    Monday Weakday = iota + 1        // 1
    Tuesday                          // 2
    Wednesday                        // 3
    ...
)
```

## Enum To String

Есть какая-то история о генераторе интерфейса stringer, но у меня не сработало: [https://medium.com/@arjunmahishi/golang-stringer-ad01db65e306](https://medium.com/@arjunmahishi/golang-stringer-ad01db65e306)

Другой способ — реализовать интерфейс Stringer таким образом:

```go
type Weakday int
const (
    Monday Weakday = iota + 1        // 1
    Tuesday                          // 2
    Wednesday                        // 3
    ...
)
func (tt Weakday) String() string {
	return [...]string{"Monday", "Tuesday", "Wednesday", }[tt]
}
```
