# OpenFeign

Declarative REST Client. OpenFeign creates a dynamic implementation of an interface decorated with JAX-RS or Spring MVC annotations.

### Configuration

Step 1: Include dependencies in pom.xml.

```xml
...
<properties>
  <java.version>11</java.version>
  <spring-cloud.version>2021.0.7</spring-cloud.version>
</properties>

<dependencies>
  <dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-openfeign</artifactId>
  </dependency>
  ...
</dependencies>

<dependencyManagement>
  <dependencies>
    <dependency>
      <groupId>org.springframework.cloud</groupId>
      <artifactId>spring-cloud-dependencies</artifactId>
      <version>${spring-cloud.version}</version>
      <type>pom</type>
      <scope>import</scope>
    </dependency>
  </dependencies>
</dependencyManagement>
...
```

Step 2: Enable Feign clients with @EnableFeignClients annotation in the main class.

```java
package br.com.romulofpaiva.hrpayroll;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.openfeign.EnableFeignClients;

@EnableFeignClients
@SpringBootApplication
public class HrPayrollApplication {

    public static void main(String[] args) {
        SpringApplication.run(HrPayrollApplication.class, args);
    }
}
```

Step 3: Configure application.properties with the client URL and path.

```properties
worker.url=http://localhost:8081
worker.path=workers
```


### Feign Client

Step 1: Create an interface for your Feign Client.

```java
package br.com.romulofpaiva.hrpayroll.clients.hrworker;

import org.springframework.cloud.openfeign.FeignClient;
import org.springframework.http.ResponseEntity;
import org.springframework.stereotype.Component;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;

@FeignClient(name = "worker", url = "${worker.url}", path = "/${worker.path}")
@Component
public interface WorkerFeignClient {

    @GetMapping(path = "/{id}")
    ResponseEntity<Worker> findById(@PathVariable Long id);

}
```

Step 2: In the service class, create a variable of the Feign client class type, then call the interface method. 

P.S.: In this example, there is an interface WorkerClient that allow us to change the client implementation without change our service class. It's a design pattern: "program to interfaces, not implementations".

```java
package br.com.romulofpaiva.hrpayroll.clients.hrworker;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;

@Component
public class WorkerClientImpl2 implements WorkerClient {

    @Autowired
    private WorkerFeignClient workerFeignClient;

    @Override
    public Worker findById(Long workerId) {
        return workerFeignClient.findById(workerId).getBody();
    }
}
```
