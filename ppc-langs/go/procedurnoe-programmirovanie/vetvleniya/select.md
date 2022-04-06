# select

В некоторых ситуациях может потребоваться запустить множество go-подпрограмм, и каждую с собственным каналом передачи данных. Контролировать взаимодействия с ними можно с помощью инструкции `select`.

## Синтаксис

```go
select {
    case передачаИлиПрием1: блок1
    ...
    case передачаИлиПриемN: блокN
    default: блокD
}
```

В инструкции select проверяется возможность выполнения каждой инструкции передачи или приема, в порядке сверху вниз. Если какие-нибудь из них могут быть выполнены (то есть не будут заблокированы), из них произвольно выбирается одна для продолжения работы.&#x20;

Если все потоки заблокированы, то, при наличии блока в default, будет вызван он и следующая инструкция за select. Если блока default нет, то будет дожидаться первый освободившийся канал.

Как вывод: с default select - не блокируется, без default - блокируется.&#x20;

## Примеры

### Пример 1

Создаем 6 небуфферезированных каналов, в которые случайным образом в цикле запихиваем значения без перерыва (и каналы сразу блокируются)

```go
channels := make([]chan bool, 6)  // array channels
for i := range channels {
    channels[i] = make(chan bool)  // init each channel
}

go func() {
    for {
        channels[rand.Intn(6)] <- true
    }
}
```

Считывает 36 булевых значений и выводим соотв номер канала

```go
for i := 0; i < 36; i++ {
    var x int
    select {
        case <-channels[0]:
            x = 1
        ...
        case <-channels[5]:
            x = 6        
    }
    fmt.Printf("%d ", x)
}
fmt.Println()
```

### Пример 2

Дорогостоящая операция

```go
func expensiveComputation(data Data, answer chan int, done chan bool) {
    ...
    finished := false
    for !finished {
        ...
        answer <- result
    }
    done <- true
}
```

Запуск двух дорогостоящих операций

```go
const allDone = 2
doneCount := 0
answer_a := make(chan int)
answer_b := make(chan int)
defer func() {
    close(answer_a)
    close(answer_b)
}()

done := make(chan bool)
defer func() { close(done) }()

go expensiveComputation(data1, answer_a, done)
go expensiveComputation(data2, answer_b, done)

for doneCount != allDone {
    var which, result int
    select {
        case result = <-answer_a:
            which = 'a'
        case result = <-answer_b:
            which = 'b'
        case <-done:
            doneCount++
    }
    if which != 0 {
        fmt.Print("%c->%d ", which, result)
    }
}
```
