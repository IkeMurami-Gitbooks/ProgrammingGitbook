# Интерфейсы

## About

Интерфейс - это тип, определяющий сигнатуры одного или более методов. Интерфейсы являются полностью абстрактными, поэтому нет никакой возможности создавать их экземпляры. Однако имеется возможность создавать переменные с типами интерфейсов, которым затем можно присваивать значения любого конкретного типа, обладающего методами интерфейса.

## Пример

### Ex1

```go
type Exchanger interface {
    Exchange()
}
type MyString struct {
    test string
}
func (mystr *MyString) Exchange() {
    mystr.test = ...
}
func (mystr *MyString) String() string {
    return mystr.test
}
// все, тип MyString реализует интерфейс Exchanger и fmt.Stringer, и если где-то будет приниматься
// тип Exchanger - можно отправлять MyString
```

### Ex2

```go
// Интерфейс
type InterfaceTest interface {
	TestFunction() (interface{}, error)
}

// Объект, соотв интерфейсу
type objectTest struct {
	fieldTest string
}

// Конструктор объекта
func ConstructorTest() InterfaceTest {
	res := objectTest{
		fieldTest: "test",
	}
	return &res
}

// Реализованный интерфейс для объекта
func (beta *objectTest) TestFunction() (interface{}, error) {
	return nil, nil
}
```

## Доступ к методам interface{}

Доступ к методам значения переданного как значение типа interface{}, можно получить, выполнив операцию приведения типа, выбора по типу или прибегнув к помощи механизма интроспекции.

### Type assertions (выбор по типу)

#### Общий вид

```go
t := i.(T)
t, ok := i.(T)
```

#### Пример

```go
package main

import "fmt"

func main() {
	var i interface{} = "hello"

	s := i.(string)
	fmt.Println(s)

	s, ok := i.(string)
	fmt.Println(s, ok)

	f, ok := i.(float64)
	fmt.Println(f, ok)

	f = i.(float64) // panic
	fmt.Println(f)
}
```

## Особенности

Принято называть интерфейсы именами, оканчивающимися на "er" - Writer, Reader, Exchanger...

Интерфейсы могут встраиваться в другие интрефейсы подобно другим типам.

Интерфейсы могут встраиваться в структуры и другие пользовательские типы: это означает, что структура может хранить любой тип, реализующий этот интерфейс.

Пустой интерфейс interface{} может использоваться для ссылки на любое значение (как Object в Java).

Неправильно выстраивать иерархию в Go из интерфейсов: правильно применять композиционный подход. То же верно и для структур.

Нормально, если интерфейс описывает одну-две функции.



