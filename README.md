# Steps for implementing the Genson library in Spring Boot REST Api

### Step1: Include the following Genson Maven dependency in the `pom.xml` 

```xml
<dependency>
    <groupId>com.owlike</groupId>
    <artifactId>genson</artifactId>
    <version>${genson.version}</version>
</dependency>

```

### Step2: Since Spring MVC has default support for Jackson and is autoconfigured with it. Hence, we must exclude Jackson dependency from the web starter in the `pom.xml`.

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

### Step3: Build a separate spring configuration class and include the following POJO databinding beans and `autowire` where it is required.
```java
@Bean
public Genson genson() {
   return new GensonBuilder()
               .setHtmlSafe(true)
               .setSkipNull(true)
               .useBeanViews(true)
               .create();
}

@Bean
public com.owlike.genson.ext.spring.GensonMessageConverter gensonMessageConverter() {
    return new com.owlike.genson.ext.spring.GensonMessageConverter(genson());
}

```
### Note: We may additionally adapt/customize the Genson depending on the use case by defining the bean with fluent API methods.

