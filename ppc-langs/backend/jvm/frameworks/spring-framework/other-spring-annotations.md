# Other Spring Annotations

## _@EnableConfigurationProperties_

Описывает простой Java-объект (POJO) со свойствами:

```java
// configuration/MyConfiguration.java
package configuration;

import org.springframework.boot.autoconfigure.AutoConfigureAfter;
import org.springframework.boot.context.properties.EnableConfigurationProperties;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import properties.MyProperties;

@Configuration
@AutoConfigureAfter(MyProperties.class)
@EnableConfigurationProperties(MyProperties.class)
public class MyConfiguration {

    @Bean
    public MyObject myObject(MyProperties properties) {
        return new MyObject(properties);
    }

}
```

И свойства:

```java
```
