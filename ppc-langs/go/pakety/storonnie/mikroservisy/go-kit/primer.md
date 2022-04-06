# Пример

## Структура микросервиса (которую я делаю)

```
pkg/
    endpoint/                tips: Endpoint Layer
        data.go              tips: Формат запросов и ответов сервера
        endpoint.go          tips: Хендлеры транспортного уровня
    repositories/            tips: Общение с persistance store
        repository.go        tips: Интерфейс CRUD низкоуровневой работы с хранилищем 
        repository_impl.go   tips: Реализация интерфейса
    service/                 tips: Service Layer
        service.go           tips: Интерфейс микросервиса: по сути описание бизнес-задачи
        service_impl.go      tips: Реализация интерфейса
    transport/               tips: Transport Layer
        transport.go         tips: Описание эндпоинтов сервера и настройка самого сервера
        util.go              tips: Encode/Decode запросов и ответов 
    stub.go                  tips: Запуск тестового сервера
```

## service

### service.go

```go
// DatabaseService describe the Database service.
type DatabaseService interface {
	GetSources(ctx context.Context) (interface{}, error)
}
```

### service\_impl.go

```go
package service

import (
	"context"
	"github.com/go-kit/kit/log"
	"some/database/pkg/repositories"
	"some/model/data"
)

type service struct {
	Repository repositories.Repository
	logger     log.Logger
}

func NewService(logger log.Logger) DatabaseService {
	return &service{
		Repository: repositories.NewRepository(),
		logger: logger,
	}
}

func (s *service) GetSources(ctx context.Context) (interface{}, error) {
	sources := s.Repository.GetSources()

	return sources, nil
}
```

### some/model/data

```go
package data

import "net/url"

type SourceType int

type Source struct {
	Url 		*url.URL 	`json:"url"`
	Type 		SourceType 	`json:"type"`
	Signature 	string 		`json:"signature,omitempty"` // commit or hash
}

// SourceList Список источников
type SourceList []Source
```

## repositories

### repository.go

```go
package repositories

import "some/model/data"

type Repository interface {
	GetSources() data.SourceList
}
```

### repository\_impl.go

```go
package repositories

import "some/model/data"

type ClientTest map[string]interface{}

func client() ClientTest {

	return map[string]interface{}{
		"sources": data.SourceList{},
	}
}

type repositoryTest struct {
	client ClientTest
}

func NewRepository() Repository {

	return &repositoryTest{
		client: client(),
	}
}

func (repo *repositoryTest) GetSources() data.SourceList {

	return repo.client["sources"].(data.SourceList)
}
```

## endpoint

### endpoint.go

```go
package endpoint

import "github.com/go-kit/kit/endpoint"

type Endpoints struct {
	GetSources endpoint.Endpoint
}

func MakeEndpoints(s service.DatabaseService) Endpoints {
	return Endpoints{
		GetSources: makeGetSourcesEndpoint(s),
	}
}

func makeGetSourcesEndpoint(s service.DatabaseService) endpoint.Endpoint {
	return func(ctx context.Context, request interface{}) (response interface{}, err error) {

		// req := request.(GetSourcesRequest)
		sources, err := s.GetSources(ctx)

		return GetSourcesResponse{Sources: sources.(data.SourceList)}, err
	}
}
```

### data.go

```go
package endpoint

type GetSourcesRequest struct {

}

type GetSourcesResponse struct {
	Sources data.SourceList `json:"sources"`
}
```

## transport

### transport.go

```go
package transport

import (
	"github.com/go-kit/kit/log"
	kithttp "github.com/go-kit/kit/transport/http"
	"github.com/gorilla/mux"
	"net/http"
)

func NewService(svcEndpoints endpoint.Endpoints, logger log.Logger) http.Handler {
	r := mux.NewRouter()
	options := []kithttp.ServerOption{

	}

	r.Methods("GET").Path("/sources").Handler(kithttp.NewServer(
		svcEndpoints.GetSources,
		decodeGetSourcesRequest,
		encodeResponse,
		options...
	))
	
	return r
}
```

### util.go

```go
package transport

func decodeGetSourcesRequest(_ context.Context, r *http.Request) (request interface{}, err error) {
	// var req endpoint.GetSourcesRequest

	return endpoint.GetSourcesRequest{}, nil

}

func encodeResponse(ctx context.Context, w http.ResponseWriter, response interface{}) error {
	if e, ok := response.(errorer); ok && e.error() != nil {
		encodeErrorResponse(ctx, e.error(), w)
		return nil
	}
	w.Header().Set("Content-Type", "application/json; charset=utf-8")
	return json.NewEncoder(w).Encode(response)
}

type errorer interface {
	error() error
}

func encodeErrorResponse(_ context.Context, err error, w http.ResponseWriter) {
	if err == nil {
		panic("encodeError with nil error")
	}

	w.Header().Set("Content-Type", "application/json; charset=utf-8")
	w.WriteHeader(codeFrom(err))
	json.NewEncoder(w).Encode(map[string]interface{}{
		"error": err.Error(),
	})
}

func codeFrom(err error) int {
	switch err {
	default:
		return http.StatusInternalServerError
	}
}
```

## stub.go

```go
package pkg

import (
	"flag"
	"github.com/go-kit/kit/log"
	"github.com/go-kit/kit/log/level"
	...
	"net/http"
	"net/url"
	"os"
)

func UpDatabaseStub() {
	// payloads := payloads()

	var (
		httpAddr = flag.String("http.addr", ":9091", "Database SVC HTTP listen address")
	)
	flag.Parse()

	var logger log.Logger
	{
		logger = log.NewLogfmtLogger(os.Stderr)
		logger = log.NewSyncLogger(logger)
		logger = log.With(logger,
			"svc",
			"database",
			"ts", log.DefaultTimestampUTC,
			"caller", log.DefaultCaller,
		)
	}

	level.Info(logger).Log("msg", "database service started")
	defer level.Info(logger).Log("msg", "database service end")

	var svc service.DatabaseService
	{
		svc = service.NewServiceStub(logger)
	}

	var h http.Handler
	{
		endpoints := endpoint.MakeEndpoints(svc)
		h = transport.NewService(endpoints, logger)
	}

	errs := make(chan error)
	go func() {
		level.Info(logger).Log("transport", "HTTP", "addr", *httpAddr)
		server := &http.Server{
			Addr: *httpAddr,
			Handler: h,
		}
		errs <- server.ListenAndServe()
	}()

	level.Error(logger).Log("exit", <-errs)
}
```
