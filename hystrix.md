# Hystrix

[Hystrix](https://github.com/Netflix/Hystrix) is a latency and fault tolerance library designed to isolate points of access to remote systems, services and 3rd party libraries, stop cascading failure and enable resilience in complex distributed systems where failure is inevitable.

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
    <artifactId>spring-cloud-starter-netflix-hystrix</artifactId>
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

Step 2: Enable Hystrix with @EnableCircuitBreaker annotation in the main class, refering to the server application name.

```java
package br.com.romulofpaiva.hrpayroll;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.client.circuitbreaker.EnableCircuitBreaker;
import org.springframework.cloud.netflix.eureka.EnableEurekaClient;
import org.springframework.cloud.openfeign.EnableFeignClients;

@EnableCircuitBreaker
@EnableEurekaClient
@EnableFeignClients
@SpringBootApplication
public class HrPayrollApplication {

    public static void main(String[] args) {
        SpringApplication.run(HrPayrollApplication.class, args);
    }
}
```

Step 3: Use the @HystrixCommand to annotate the method that you want to define a fallback strategy.

```
    @HystrixCommand(fallbackMethod = "getPaymentFallback")
    @GetMapping("/{workerId}/days/{days}")
    public Payment getPayment(@PathVariable Long workerId, @PathVariable Integer days) {
        return service.getPayment(workerId, days);
    }

    public Payment getPaymentFallback(Long workerId, Integer days) {
        return new Payment("Not found", 1.0, 1);
    }

```


Step 4: Configure application.properties with the maximum acceptable timeout for your system.

```properties
hystrix.command.default.execution.isolation.thread.timeoutInMilliseconds=2000
ribbon.ConnectTimeout=3000
ribbon.ReadTimeout=4000
```

