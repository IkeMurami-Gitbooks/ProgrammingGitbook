# HTTP-сервер

```go
func main() {
    http.HandleFunc("/", homePage)
    if err := http.ListenAndServe(":9001", nil); err != nil {
        log.Fatal("failed to start server", err)
    }
}

func homePage(writer http.ResponseWriter, request *http.Request) {
    err := request.ParseForm()  // должна вызываться перед записью в ответ
    fmt.Fprint(writer, pageTop, form)  // form = '<form>...</form>'
}
```

`HandleFunc(path, callback)`, callback должен иметь сигнатуру `func(http.ResponseWriter, *http.Request)`.

ListenAndServe(address, serverType), serverType=nil -> choose default

Другой вариант: [https://github.com/projectdiscovery/simplehttpserver](https://github.com/projectdiscovery/simplehttpserver)

