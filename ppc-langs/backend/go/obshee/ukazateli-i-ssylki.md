# Указатели и ссылки

Есть указатели, а есть ссылки. Ссылки обычно автоматом создаются на срезы, на функции, на каналы. Ссылки не надо разыменовывать. Ссылки можно  передавать как значения. Изменение значения по ссылке внутри какой-то функции приведет к изменению значения и вне ее (значения не копируются при передаче ссылки в функцию).

## Указатели

Все то же, что и в C. Однако арифметические операции над указателями исключены.

```go
func (test *Test) some() {  // передаем по указателю
    *test = ...    // Разыменование указателя (получение доступа к значению по указателю)
}
```

Важно понимать, что если метод применяется к указателю и принимает значение, компилятор Go **поймет**, что нужно сделать разыменование. Если метод применяется к значению и принимает указатель, компилятор Go **поймет**, что нужно передать указатель.

При обращении к полю структуры, структура разыменовывается автоматически: не надо разыменовывать ее отдельно.

### Создание переменных и получение указателей на них

Есть два способа - через оператор & и через new():

```go
type composer struct { 
    name string
    num int 
}

test1 := composer{"test", 1}  // значение типа

test2 := new(composer)    // указатель на значение типа
test2.name, test2.num = "test", 1

test3 := &composer{}    // указатель на значение типа
test3.name, test3.num = "test", 1

test4 := &composer{"test", 1} // указатель на значение типа 
```



### Операторы работы с указателями

```go
pi := &i  // взятие указателя
i = *pi   // разыменоновывание указателя (еще называется доступом к содержимому или косвенной адресацией)

*int - этот тип называется "указатель на int"
```

## Ссылки

Каналы, отображения и срезы в языке Go создаются с помощью функции `make()`, которая всегда возвращает ссылку. Ссылка действует почти так же, как и указатель. Однако, ссылки не требуется разыменовывать (в большинстве случаев не требуется добавлять звездочку в конец).
