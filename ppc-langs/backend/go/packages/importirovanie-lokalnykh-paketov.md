# Импортирование локальных пакетов

## Нюансы

* В одной папке могут быть скрипты только одного пакета
*

## Примеры

### Импортирование локальных пакетов

```go
package main

/*
.. /examples
.. .../test.go
.. main.go
*/

import test "./examples"

func main() {
    test.TestFunc()
}
```
