# context

Source: [https://habr.com/ru/company/nixys/blog/461723/](https://habr.com/ru/company/nixys/blog/461723/)

## Контекст (Context)

Пакет **context** в go позволяет вам передавать данные в вашу программу в каком-то «контексте». Контекст так же, как и таймаут, дедлайн или канал, сигнализирует прекращение работы и вызывает return.

Для примера, если вы делаете веб-запрос или выполняете системную команду, будет хорошей идеей использовать таймаут для production-grade систем. Потому что, если API, к которому вы обращаетесь, работает медленно, вы вряд ли захотите накапливать запросы у себя в системе, поскольку это может привести к увеличению нагрузки и снижению производительности при обработке собственных запросов. В результате возникает каскадный эффект.

И здесь как раз может пригодиться контекст тайм-аута или дедлайна.

## Создание контекста

Пакет context позволяет создавать и наследовать контекст следующими способами:

### Background

Эта функция возвращает пустой контекст. Она должна использоваться только на высоком уровне (в main или обработчике запросов высшего уровня). Он может быть использован для получения других контекстов, которые мы обсудим позже.

<pre class="language-go"><code class="lang-go">// Background returns an empty Context. It is never canceled, has no deadline,
// and has no values. Background is typically used in main, init, and tests,
// and as the top-level Context for incoming requests.
<strong>ctx := context.Background()
</strong></code></pre>

### TODO

Это идентичный Background'у контекст, но называется по другому. Он нужен, когда мы еще не знаем какой контекст будем встраивать (когда определимся, ищем по коду и меняем на нужный контекст):

```go
ctx := context.TODO()
```

## Порождение контекста

От Background и других контекстов порождаем нужные нам контексты:

```go
// WithCancel returns a copy of parent whose Done channel is closed as soon as
// parent.Done is closed or cancel is called.
func WithCancel(parent Context) (ctx Context, cancel CancelFunc)

// A CancelFunc cancels a Context.
type CancelFunc func()

// WithTimeout returns a copy of parent whose Done channel is closed as soon as
// parent.Done is closed, cancel is called, or timeout elapses. The new
// Context's Deadline is the sooner of now+timeout and the parent's deadline, if
// any. If the timer is still running, the cancel function releases its
// resources.
func WithTimeout(parent Context, timeout time.Duration) (Context, CancelFunc)

// WithValue returns a copy of parent whose Value method returns val for key.
func WithValue(parent Context, key interface{}, val interface{}) Context
```

###
