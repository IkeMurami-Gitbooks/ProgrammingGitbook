# Libraries

## Lombok

Библиотека позволяет удобно настраивать логирование в коде

Link: [https://projectlombok.org/](https://projectlombok.org/)

## Reactor

Реактивное программирование поддерживается Spring Framework, начиная с версии 5. Эта поддержка построена на основе Project Reactor.

Link: [https://projectreactor.io/](https://projectreactor.io/)

Project Reactor (или просто Reactor) - это библиотека Reactive для создания неблокирующих приложений на JVM, основанная на спецификации Reactive Streams. Reactor - это основа реактивного стека в экосистеме Spring, и он разрабатывается в тесном сотрудничестве со Spring. WebFlux, веб-фреймворк с реактивным стеком Spring, использует Reactor в качестве базовой зависимости.

Проект Reactor состоит из набора модулей, перечисленных в документации Reactor. Модули встраиваемы и совместимы. Основным артефактом является Reactor Core, который содержит реактивные типы **Flux** и **Mono**. И Flux и Mono — publishers. Mono может отправить только один элемент, Flux сколько хочет.

Подробнее на верхнем уровне про Reactor: [https://habr.com/ru/post/565050/](https://habr.com/ru/post/565050/)
