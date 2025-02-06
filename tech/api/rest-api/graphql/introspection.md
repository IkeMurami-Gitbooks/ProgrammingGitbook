---
description: Извлечение информации и типов
---

# Introspection

## Playground

Песочница от Apollo: [https://studio.apollographql.com/sandbox/explorer](https://studio.apollographql.com/sandbox/explorer)

Можно указать путь до эндпоинта и в UI смотреть запросы

## Examples

### Example1

Обнаружив инстанс, нужно понять какие запросы поддерживает graphql: [https://graphql.org/learn/introspection/](https://graphql.org/learn/introspection/)\
ex: `example.com/graphql?query={__schema{types{name,fields{name}}}`

### Example2

```
Запрос
POST /graphql
...

{
    "operationName":"types",
    "variables":{},
    "query":"query {__schema{types{name,fields{name}}}}"
}

В ответ - ошибка
{
    "errors": [
        {
            "message": "Unknown operation named \"types\".",
            "extensions": {
                "..."
            }
        }
    ]
}
```

Почему так произошло, и как сделать интроспекцию в этом случае:\
Почему: есть разные либы для graphql, некоторые игнорируют operationName, другие - нет.\
Решение: тут надо было в operationName указать запрос на интроспекцию - IntrospectionQuery

Пример запроса на интроспекцию в этом случае будет выглядеть так (параметр variables не важен здесь):

```
{
    "operationName": "IntrospectionQuery",
    "variables": {
        "pageNumber:1,
        "itemsPerPage":10,
        "name":""
    },
    "query":"fragment FullType on __Type { kind name ..."
}
```

## Papers

Про интроспекцию (извлечение информации и типов): [https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/GraphQL%20Injection](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/GraphQL%20Injection)

## Tools

[https://github.com/sorokinpf/graphql\_sample\_sender](https://github.com/sorokinpf/graphql_sample_sender)
