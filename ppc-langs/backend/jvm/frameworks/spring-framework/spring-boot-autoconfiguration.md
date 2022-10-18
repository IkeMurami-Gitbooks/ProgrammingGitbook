# Spring Boot AutoConfiguration

Ключ к волшебству автоконфигурации Spring Boot — аннотация **@EnableAutoConfiguration**. Обычно мы аннотируем наш класс, являющейся точкой входа в приложение, либо с помощью **@SpringBootApplication**, либо, если мы хотим настроить значения по умолчанию, мы можем использовать следующие аннотации:

```java
@Configuration
@EnableAutoConfiguration
@ComponentScan
public class Application
{

}
```

Аннотация **@EnableAutoConfiguration** включает автоматическую настройку Spring **ApplicationContext** путем сканирования компонентов пути к классам и регистрации бинов, соответствующих различным условиям.

Обычно классы **AutoConfiguration** аннотируются **@Configuration**, чтобы пометить его как класс конфигурации Spring, и аннотируются **@EnableConfigurationProperties** для привязки свойств настройки и одного или нескольких методов регистрации условного компонента.

Пример [смотри в статье с хабра](https://habr.com/ru/post/487980/) про автоконфигурацию.
