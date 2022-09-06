# About

На моем [Github](https://github.com/IkeMurami/zdocker-env/tree/master/JVM) можно найти маленькие примеры приложений на Spring Framework.

Spring: [https://spring.io/](https://spring.io/)\
Tutorial: [https://kotlinlang.org/docs/jvm-spring-boot-restful.html](https://kotlinlang.org/docs/jvm-spring-boot-restful.html)

## Примерная зависимость компонентов:

```
Configurator — инициализирует HttpClient, Service, Controller, ...
Controller — принимает Service, обрабатывает API вызовы и передает сервису
Service — уровень логики, берет на себя таски в фоне
```

## В коде

### Configurator

```java
// ...

@Configurator
public class SomeContextConfigurator {

    @Bean
    public SomeController someController( ... ) {
        return new SomeController(someService, ...);
    }

    @Bean
    public SomeService someService(
        ...
    ) {
        return new SomeService(...);
    }

    @Bean
    public CloseableHttpClient httpClient(
        ...
    ) {
        ApacheHttpClientUtils.Builder builder = ApacheHttpClientUtils.Builder.create()
            .multiThreaded()
            . // ...
        
        return builder.build();
    }
}

// ...
```

### Controller

```java
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;


@RestController
@RequestMapping(value = "/some")
public class SomeController {

    private final SomeService someService;

    public SomeController(
        SomeService someService
    ) {
        this.someService = someService;
    }

    @RequestMapping(value = "send", method = RequestMethod.POST)
    public SomeResult someAction(
        @RequestParam("post_param") String postParam
    ) {
        // ...
        someService.someAction(postParam);
        // ...
    }

}
```

### Service

Interface

```java
public interface SomeService {

    SomeResult someAction(String postParam);

}
```

Impl

```java
public class ConnectorSendSmsServiceImpl implements ConnectorSendSmsService {

    // ...
    
}
```
