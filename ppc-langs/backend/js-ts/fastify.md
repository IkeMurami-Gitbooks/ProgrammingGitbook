# Fastify

Применяют как HTTP server side app (например, за него можно поставить уже ваше приложение, а можно использовать как свое Server App)

Link: [https://github.com/fastify/fastify](https://github.com/fastify/fastify)

## Security

### Validation and Serialization

У Fastify есть возможность на лету работать с объектами, однако это небезопасная фича (разработчиков об этом предупреждают): [https://www.fastify.io/docs/latest/Reference/Validation-and-Serialization/](https://www.fastify.io/docs/latest/Reference/Validation-and-Serialization/))

### Prototype Poisoning

Как и любое JavaScript приложение, Fastify подвержен Prototype Poisoning атаке: использовать `JSON.parse` для внешних объектов — потенциальный риск.
