# Ribbon

Ribbon is a client side IPC library that is battle-tested in cloud. It provides the following features:

- Load balancing
- Fault tolerance
- Multiple protocol (HTTP, TCP, UDP) support in an asynchronous and reactive model
- Caching and batching

### Configuration

Step 1: Include dependencies in pom.xml.

```xml
...
<parent>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-parent</artifactId>
  <version>2.3.4.RELEASE</version>
  <relativePath/> <!-- lookup parent from repository -->
</parent>
...
<properties>
  <java.version>11</java.version>
  <spring-cloud.version>Hoxton.RELEASE</spring-cloud.version>
</properties>

<dependencies>
  <dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-ribbon</artifactId>
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

Step 2: Enable Ribbon client with @RibbonClient annotation in the main class, refering to the server application name.

```java
package br.com.romulofpaiva.hrpayroll;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.netflix.ribbon.RibbonClient;
import org.springframework.cloud.openfeign.EnableFeignClients;

@RibbonClient(name = "hr-worker")
@EnableFeignClients
@SpringBootApplication
public class HrPayrollApplication {

    public static void main(String[] args) {
        SpringApplication.run(HrPayrollApplication.class, args);
    }
}
```

Step 3: Configure application.properties with the list of servers URL and path.

```properties
hr-worker.ribbon.listOfServers=http://localhost:8001,http://localhost:8002
hr-worker.path=workers
```


### Feign Client

Step 1: Update the interface of your Feign Client, removing the URL.

```java
package br.com.romulofpaiva.hrpayroll.clients.hrworker;

import org.springframework.cloud.openfeign.FeignClient;
import org.springframework.http.ResponseEntity;
import org.springframework.stereotype.Component;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;

@FeignClient(name = "hr-worker", path = "/${hr-worker.path}")
@Component
public interface WorkerFeignClient {

    @GetMapping(path = "/{id}")
    ResponseEntity<Worker> findById(@PathVariable Long id);

}
```

