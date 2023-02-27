# Problem Statement
We’re going to assume we already have a simple REST API built with Spring Boot, correctly implemented, and able to serve JSON with Jackson.
Let’s say you now had to serve JSON replacing Jackson with a library like Genson: https://github.com/owlike/genson - what would you implement to be able to do that? Let’s specifically focus on the Spring config aspect of this implementation, of course.

## Steps for implementing the Genson library in Spring Boot REST Api

### Step1 : Include the following Genson Maven dependency in the `pom.xml` and Gradle project, we should add the in the `build.gradle`

```xml
<dependency>
    <groupId>com.owlike</groupId>
    <artifactId>genson</artifactId>
    <version>${genson.version}</version>
</dependency>

<!--Extension depedency for customizing date and time -->
<dependency>
    <groupId>com.owlike</groupId>
    <artifactId>genson-java-datetime</artifactId>
    <version>${genson.version}</version>
 </dependency>

```
For Gradle Project
```
implementation group: 'com.owlike', name: 'genson', version: '1.6'
```

### Step2 : Since Spring MVC has default support for Jackson and is autoconfigured with it. Hence, we must `exclude Jackson dependency` from the web starter in the `pom.xml`.

```xml
<dependency>
     <groupId>org.springframework.boot</groupId>
     <artifactId>spring-boot-starter-web</artifactId>
     <exclusions>
         <exclusion>
             <groupId>org.springframework.boot</groupId>
             <artifactId>spring-boot-starter-json</artifactId>
         </exclusion>
      </exclusions>
</dependency>

```

### Step3 : Build a separate spring configuration class and include the following POJO databinding beans and `autowire` where it is required.
```java
import com.owlike.genson.Genson;
import com.owlike.genson.GensonBuilder;
import com.owlike.genson.ext.spring.GensonMessageConverter;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class AppConfig {

    // Several additional built-in fluent api methods may be defined.
    @Bean
    public Genson genson() {
        return new GensonBuilder()
                .setHtmlSafe(true)
                .setSkipNull(true)
                .useBeanViews(true)
                .withBundle(new JavaDateTimeBundle())
                .create();

    }

    @Bean
    public GensonMessageConverter gensonMessageConverter() {
        return new GensonMessageConverter(genson());
    }
}

```
### Note : We may additionally adapt/customize the Genson depending on the use case by defining the bean with fluent API methods.

Example of the code: https://github.com/bsmahi/genson-springboot

