# How to configure Eureka Server


### Configuration

Step 1: Using https://start.spring.io/, create a simple project with the unique dependency "Eureka Server". Download the code, unzip the file, and open the project in an IDE (Integrated Development Environment) like IntelliJ or Spring Tools Suite (STS).

Step 2: Update the main application class, adding the annotation @EnableEurekaServer.

```
@EnableEurekaServer
@SpringBootApplication
public class HrEurekaServerApplication {

	public static void main(String[] args) {
		SpringApplication.run(HrEurekaServerApplication.class, args);
	}

}
```

Step 3: Add the following properties to the application.properties.

```
spring.application.name=hr-eureka-server
server.port=8761

eureka.client.register-with-eureka=false
eureka.client.fetch-registry=false
```

Step 4: Run the project, access the URL http://localhost:8761/, and you should see an page like that.

![alt text][eureka]

[eureka]: eureka.png "Eureka Server page in localhost"
