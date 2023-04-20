# Введение

## Общее

GraphQL - язык запросов к данным (разных типов, разных хранилищ и тп), разработанный Facebook\
По дефолту, аутентификация не реализована: разработчик должен ее сам накрутить.\
При бруте директорий, можно добавить следующие пути для детекта graphQL инстансов:\
`/graphql`\
`/graphiql`\
`/graphql.php`\
`/graphql/console`\
`/admin-graphql-api`\
`/main-graphql-api`

## О GraphQL как языке

Состоит из трех "строительных" блоков:\
\- схема (schema)\
\- запросы (queries)\
\- распознаватели (resolvers)

### Запросы

Пример запросов

```
Запрос данных: 

query {
  repository(owner: "graphql", name: "graphql-js"){
    name
    description
  }
}

Вызов функции:
query getMyPost($id: String) {
  post(id: $id){
    title
    body
    author{
      name
      avatarUrl
      profileUrl
    }
  }
}
```

![Общий вид запросов](<../../../../.gitbook/assets/изображение (6).png>)

