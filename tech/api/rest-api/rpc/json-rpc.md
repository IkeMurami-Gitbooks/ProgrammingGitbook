# JSON RPC

JSON-RPC 2.0\
Спецификация: [https://www.jsonrpc.org/specification](https://www.jsonrpc.org/specification)

Краткое описание\
1 Запрос включает 4 поля:\
jsonrpc - всегда будет “2.0”, указывает версию протокола.\
method - название метода (функции), который нужно вызвать.\
params — опциональное поле, нагрузка к вызову (аргументы функции).\
id — опциональное поле, уникальный идентификатор вызова. Если вы хотите получить значение от вызванной функции, то вы должны сгенерировать id на стороне клиента и при ответе вы сможете понять, на какой именно вызов пришел ответ, сопоставив id ответа.

Если вы не отправили id, то это означает, что ответ вас не интересует и от сервера вы ничего не получите. Такой вызов называется нотификацией.

2 Ответ\
jsonrpc — всегда будет “2.0”, указывает версию протокола.\
result — тело ответа (возвращаемое значение функции).\
id — уникальный идентификатор ответа. Он нужен для того, чтобы клиент мог сопоставить, на какой запрос он получил ответ.\
error — в случае ошибки вместо result, вы получите поле error, содержащее в себе code (код ответа: по протоколу их может быть шесть) и message (человекопонятное описание ошибки).

3 Batch\
Можно делать несколько запросов в одном

4 Коды ошибок\
code message meaning\
\-32700 Parse error Invalid JSON was received by the server.\
An error occurred on the server while parsing the JSON text.\
\-32600 Invalid Request The JSON sent is not a valid Request object.\
\-32601 Method not found The method does not exist / is not available.\
\-32602 Invalid params Invalid method parameter(s).\
\-32603 Internal error Internal JSON-RPC error.\
\-32000 to -32099 Server error Reserved for implementation-defined server-errors.

5 Пример реализации\
[https://www.npmjs.com/package/express-json-rpc-router](https://www.npmjs.com/package/express-json-rpc-router)
