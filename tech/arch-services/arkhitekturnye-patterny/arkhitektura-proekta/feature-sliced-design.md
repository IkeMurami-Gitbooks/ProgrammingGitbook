# Feature-Sliced Design

Link: [https://feature-sliced.design/ru](https://feature-sliced.design/ru)

## Отдельные заметки по концепции

### Директории

* app — управление контекстом, аналитикой, стилями, роутами
* (deprecated) processes
* pages — страницы и экраны, готовые подключаться к роутам
* widgets — блоки пользовательского интерфейса
* features — действия над entities, бизнес-логика
* entities — CRUD/UI/хранение данных
* shared — не связанные со спецификой проекта кусочки: UI-библиотеки, API-клиенты, работа с Browser API

Еще есть понятие слайсов (slice). app и shared слайсов не содержат. Остальные директории строятся на слайсах.

Именование общее директорий:

```
feature/
   sliceN/
      ui
      model
      lib
      api
```

