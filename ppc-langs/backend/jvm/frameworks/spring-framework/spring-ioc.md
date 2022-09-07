# Spring IoC

IoC — Inversion of Control, так же известное как Dependency Injection (DI) — процесс, согласно которому объекты определяют свои зависимости, т.е. объекты, с которыми они работают, через аргументы конструктора/фабричного метода или свойства, которые были установлены или возвращены фабричным методом. Затем контейнер _inject_(далее "внедряет") эти зависимости при создании бина. Этот процесс принципиально противоположен, поэтому и назван _Inversion of Control_, т.к. бин сам контролирует реализацию и расположение своих зависимостей, используя прямое создание классов или такой механизм, как шаблон _Service Locator_.

Основными пакетами Spring Framework IoC контейнера являются `org.springframework.beans` и `org.springframework.context`. Интерфейс `BeanFactory` предоставляет механизм конфигурации по управлению любым типом объектов. `ApplicationContext` - наследует нитерфейс `BeanFactory` и добавляет более специфичную функциональность.

**Замечение**: Аннотации `@Autowired`, `@Inject`, `@Resource` и `@Value` обрабатываются Spring реализацией `BeanPostProcessor`, поэтому вы не можете их применять в своих собственных `BeanPostProcessor` и `BeanFactoryPostProcessor`, а только лишь явной инициализацией через XML или `@Bean` метод.

## Настройка IoC контейнера

### @Configuration & @Bean

Основными признаками и частями Java-конфигурации IoC контейнера являются классы с аннотацией `@Configuration` и методы с аннотацией `@Bean`. Аннотация `@Bean` используется для указания того, что метод создает, настраивает и инициализирует новый объект, управляемый Spring IoC контейнером. Такие методы можно использовать как в классах с аннотацией `@Configuration`, так и в классах с аннотацией `@Component`(или её наследниках). Класс с аннотацией `@Configuration` говорит о том, что он является источником определения бинов. Самая простейшая из возможных конфигураций выглядит следующим образом:

```java
package lessons;

import org.springframework.context.annotation.Configuration;

/**
 * Конфигурационный класс Spring IoC контейнера
 */
@Configuration
public class LessonsConfiguration {
}
```

Для того, чтобы приступить к настройке и изучению Spring IoC контейнера, вы должны инициализировать `ApplicationContext`, который поможет также с разрешением зависимостей. Для обычной Java-конфигурации применяется `AnnotationConfigApplicationContext`, в качестве аргумента к которому передается класс, либо список классов с аннотацией `@Configuration`, либо с любой другой аннотацией JSR-330, в том числе и `@Component`:

```java
public class Starter {

    private static final Logger logger = LogManager.getLogger(Starter.class);

    public static void main(String[] args) {
        logger.info("Starting configuration...");

        ApplicationContext context = new AnnotationConfigApplicationContext(LessonsConfiguration.class);
    }
}

```

### @Import

Большая часть приложений строится по модульной архитектуре, разделенная по слоям, например DAO, сервисы, контроллеры и др. Создавая конфигурацию, можно также её разбивать на составные части, что также улучшит читабельность и панимание архитектуры вашего приложения. Для этого в конфигурацию необходимо добавить аннотацию `@Import`, в параметрах которой указываются другие классы с аннотацией `@Configuration`, например:

```java
// src/main/java/lessons/AnotherConfiguration.java
@Configuration
public class AnotherConfiguration {
    @Bean
    BeanWithDependency beanWithDependency() {
        return new BeanWithDependency();
    }
}

// src/main/java/lessons/LessonsConfiguration.java
@Configuration
@ComponentScan
@Import(AnotherConfiguration.class)
public class LessonsConfiguration {
    @Bean
    GreetingService greetingService() {
        return new GreetingServiceImpl();
    }
}
```

Таким образом, при инициализации контекста вам не нужно дополнительно указывать загрузку из конфигурации `AnotherConfiguration`, все останется так, как и было:

```java
public class Starter {

    private static final Logger logger = LogManager.getLogger(Starter.class);

    public static void main(String[] args) {
        logger.info("Starting configuration...");

        ApplicationContext context = new AnnotationConfigApplicationContext(LessonsConfiguration.class);
        GreetingService greetingService = context.getBean(GreetingService.class);
        BeanWithDependency withDependency = context.getBean(BeanWithDependency.class);
        logger.info(greetingService.sayGreeting()); // "Greeting, user!"
        logger.info(withDependency.printText()); // "Some text!"
    }
}
```

### @Autowired

В большинстве случаев, имеются такие случаи, когда бин в одной конфигурации имеет зависимость от бина в другой конфигурации. Поскольку конфигурация является источником определения бинов, то разрешить такую зависимость не является проблемой, достаточно объявить поле класса конфигурации с аннотацией `@Autowired`(более подробно описано в отдельной главе):

```java
// src/main/java/lessons/AnotherConfiguration.java
@Configuration
public class AnotherConfiguration {

    @Autowired GreetingService greetingService;

    @Bean
    BeanWithDependency beanWithDependency() {
        //что-нибудь делаем с greetingService...
        return new BeanWithDependency();
    }
}

// src/main/java/lessons/LessonsConfiguration.java
@Configuration
@ComponentScan
@Import(AnotherConfiguration.class)
public class LessonsConfiguration {
    @Bean
    GreetingService greetingService() {
        return new GreetingServiceImpl();
    }
}
```

### XML конфигурации

Классы с аннотацией `@Configuration` не стремятся на 100% заменить конфигурации на XML, при этом, если вам удобно или имеется какая-то необходимость в использовании XML конфигурации, то к вашей Java-конфигурации необходимо добавить аннотацию `@ImportResource`, в параметрах которой необходимо указать нужное вам количество XML-конфигураций. Выглядит это следующим способом:

```java
// src/main/java/lessons/LessonsConfiguration.java
@Configuration
@ImportResource("classpath:/lessons/xml-config.xml")
public class LessonsConfiguration {
    @Value("${jdbc.url}")
    String url;
    //...
}

// src/main/java/lessons/xml-config.xml
<beans>
    <context:property-placeholder location="classpath:/jdbc.properties"/>
</beans>

// jdbc.properties
jdbc.url=jdbc:hsqldb:hsql://localhost/xdb
```
