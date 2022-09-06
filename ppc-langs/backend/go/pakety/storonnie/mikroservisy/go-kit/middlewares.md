# Middlewares

Middlewares в Go kit предоставляет мощный механизм, с помощью которого можно обернуть сервисы и эндпоинты и добавить функциональность (изолированные компоненты), такие как логирование, прерывание запросов, ограничение количества запросов, балансировку нагрузки или распределенную трассировку.

```go
type Middleware func(Endpoint) Endpoint
```

(В go-kit этот тип уже предоставляется. В промежутке он может делать все, что угодно. Ниже вы можете увидеть, как можно реализовать базовое промежуточное ПО для ведения журнала (вам не нужно никуда копировать / вставлять этот код)):

```go
func loggingMiddleware(logger log.Logger) Middleware {
	return func(next endpoint.Endpoint) endpoint.Endpoint {
		return func(ctx context.Context, request interface{}) (interface{}, error) {
			logger.Log("msg", "calling endpoint")
			defer logger.Log("msg", "called endpoint")
			return next(ctx, request)
		}
	}
}
```
