# RestTemplate

[RestTemplate](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/client/RestTemplate.html) is a synchronous client to perform HTTP requests, exposing a simple, template method API over underlying HTTP client libraries such as the JDK HttpURLConnection, Apache HttpComponents, and others. RestTemplate offers templates for common scenarios by HTTP method, in addition to the generalized exchange and execute methods that support less frequent cases.


### Configuration

Step 1: Include dependencies in pom.xml.

```xml
...
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-web</artifactId>
</dependency>
...
```

Step 2: Create a configuration class, define a method annoted with @Bean and that returns a new RestTemplate instance.

```java
package br.com.romulofpaiva.hrpayroll.clients.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.client.RestTemplate;

@Configuration
public class ConfigRestTemplate {

    @Bean
    public RestTemplate createRestTemplate() {
        return new RestTemplate();
    }

}
```

Step 3: Configure application.properties with the client URL and path.

```properties
worker.url=http://localhost:8081
worker.path=workers
```


### RestTemplate Client

In the service class, create a variable of the RestTemplate client class type, then call the interface method. 

P.S.: In this example, there is an interface WorkerClient that allow us to change the client implementation without change our service class. It's a design pattern: "program to interfaces, not implementations".

```java
package br.com.romulofpaiva.hrpayroll.clients.hrworker;

import java.util.HashMap;
import java.util.Map;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;
import org.springframework.web.client.RestTemplate;

@Component
public class WorkerClientImpl1 implements WorkerClient {

    @Autowired
    private RestTemplate restTemplate;

    @Value("${worker.url}")
    private String workerUrl;

    @Value("${worker.path}")
    private String workerPath;

    @Override
    public Worker findById(Long workerId) {
        Map<String, String> uriVariables = new HashMap<>();
        uriVariables.put("id", String.valueOf(workerId));

        return restTemplate.getForEntity("http://localhost:8081/workers/{id}", Worker.class, uriVariables).getBody();
    }

}
```
