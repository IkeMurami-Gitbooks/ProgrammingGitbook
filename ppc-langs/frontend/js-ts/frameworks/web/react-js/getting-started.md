# Getting Started

Создаем проект на чистом React

```
$ npx create-react-app test-project-name
$ cd test-project-name
$ yarn start (or npm start)
```

В этом случае мы сами решаем вопросы:

* Routing, например:
  * [react-router](https://reactrouter.com/start/data/custom)
  * [tanstack router](https://tanstack.com/router/latest)
* Data Fetching (в чем сложности: отслеживание состояния загрузки, ошибок, кэширование, чтобы дважды не ходить за данными)
  * rest api
    * [TanStack Query](https://tanstack.com/query/)
    * [SWR](https://swr.vercel.app/)
    * [RTK Query](https://redux-toolkit.js.org/rtk-query/overview)
  * graphql
    * [Apollo](https://www.apollographql.com/docs/react)
    * [Relay](https://relay.dev/)
* Code-splitting — разбиваем бандл на маленькие бандлы, чтобы они быстрее подгружались и тогда, когда это действительно надо
  *

