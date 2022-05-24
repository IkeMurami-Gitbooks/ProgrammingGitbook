# Spring Boot

Уязвимости в Spring Boot [https://github.com/pyn3rd/Spring-Boot-Vulnerability](https://github.com/pyn3rd/Spring-Boot-Vulnerability)

## Spring Boot Actuator

Spring Boot Actuator — на сколько понял, это инструмент/фреймворк для отслеживания состояния приложения. Вот простой  пример уязвимого приложения: [https://github.com/veracode-research/actuator-testbed](https://github.com/veracode-research/actuator-testbed)

Статья к этому приложению: [https://www.veracode.com/blog/research/exploiting-spring-boot-actuators](https://www.veracode.com/blog/research/exploiting-spring-boot-actuators)

Actuator API: [https://docs.spring.io/spring-boot/docs/2.0.x/actuator-api/html/](https://docs.spring.io/spring-boot/docs/2.0.x/actuator-api/html/)

Из env можем достать env'ы приложения. Из heapdump скачиваем дамп процесса и с помощью VisualVM ищем секреты.

В зависимости от данных, за это могут накинуть порядочно: [https://hackerone.com/reports/1022048](https://hackerone.com/reports/1022048)

В общем, если в actuator включено все, конечно, это можно запавнить, но по умолчанию это все отключено. Пример небезопасной конфигурации в `application.properties`:

```
management.endpoint.env.post.enabled=true
management.endpoint.restart.enabled=true
endpoints.sensitive=true
endpoints.actuator.enabled=true
management.security.enabled=false 
management.endpoints.web.exposure.include=*
```
