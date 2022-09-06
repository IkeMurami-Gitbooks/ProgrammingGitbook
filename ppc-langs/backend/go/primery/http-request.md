# Test HTTP Request

Тестовый http-запрос на указанный хост/порт и как его собрать под винду

```go
package main

import "net/http"

// Build: GOOS=windows GOARCH=amd64 go build -o check_443.exe

func main() {
	client := http.Client{}
	client.Get("http://192.168.50.158:443/test")
}

```
