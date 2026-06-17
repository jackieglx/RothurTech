# Recording links for Day16 - 06/17/2026 Mock

https://xiao-java-05182026-demo-bucket.s3.us-west-2.amazonaws.com/mock-interview/2026-06-17-09-45-06.mp4?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAW3MEATYNQS2XOAZB%2F20260617%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260617T165654Z&X-Amz-Expires=604800&X-Amz-SignedHeaders=host&X-Amz-Signature=b7b24db4d4cc75a0a49c50e53ea2e61271f81ac98b5d7f7957450aba230447ea



## if we override hashcode not overriding equals

If we override `hashCode()` but not `equals()`, logically equal objects will be treated as different keys after a hash collision.

For hash-based data structures, adding or updating a logically equal key will create a new entry instead of updating the existing value. This can cause duplicate logical keys in `HashMap` or `HashSet`.

Therefore, when we override `hashCode()`, we should always override `equals()` as well to maintain their contract.





## how do you write restapi in spring boot

RESTful API means we use URLs to represent resources and HTTP methods to represent actions.

One important REST principle is that URLs should use nouns instead of verbs.

We can also include the API version in the URL, such as `/api/v1/students`, so we can introduce new versions without breaking existing clients.

To write REST APIs in Spring Boot, I create a controller class with `@RestController`annotation which is equivalent to `@Controller` plus `@ResponseBody`. 

Then I use `@RequestMapping` to define the base URL.

Inside the class, we use annotations such as `@GetMapping` and `@PostMapping` on methods to handle different HTTP requests.

I use `@PathVariable` to read values from the URL, `@RequestParam` to get query parameters, and `@RequestBody` to read JSON data from the request body. 

For the logic inside controller methods, the controller usually calls the service layer. The service layer handles the business logic and calls the repository layer to interact with the database.

For responses, I use `ResponseEntity` to wrap the DTO because it allows me to set the HTTP status code, response headers, and response body.



## what is thread state

There are 6 Java thread states, including `NEW`, `RUNNABLE`, `BLOCKED`, `WAITING`, `TIMED_WAITING`, and `TERMINATED`.

- A thread is in `NEW` state after we create a `Thread` object, but we haven't called the`start()`.
- After we call `start()`, the thread enters `RUNNABLE` state. `RUNNABLE` means the thread is ready to run or is actually running. The real execution depends on the JVM and OS thread scheduler
- A thread enters `BLOCKED` state when it tries to enter a `synchronized` block or method, but another thread already holds the monitor lock. When the lock is released and this thread gets the lock, it goes back to `RUNNABLE`.
- A thread enters `WAITING` state when it calls methods like `wait()` or `join()` without timeout. For `wait()`, the thread releases the monitor lock and waits for `notify()` or `notifyAll()`. After being notified, it turns to BLOCKED state and is able to compete for the lock again. After it gets the lock, it goes back to `RUNNABLE`.
- A thread enters `TIMED_WAITING` state when it calls methods with a timeout, such as `sleep(time)`, `wait(time)`, or `join(time)`. 
- A thread enters `TERMINATED` state when its `run()` method finishes or throws an uncaught exception.





## what is thread local

`ThreadLocal` provides a separate copy of a variable for each thread, so threads do not share that value. 

 It is useful when data should belong to the current thread, such as a user ID, trace ID, or request context. 

 Internally, each `Thread` contains a `ThreadLocalMap`, and the `ThreadLocal` object is used as the key to store and retrieve the value. When we call `set()` method, it stores the value in the current thread’s map. When we call `get()`, it reads the value from that same map.

Some common use cases for `ThreadLocal` include storing authentication information, transaction context, request metadata, and logging trace IDs.

We should call `remove()` in a `finally` block because pooled threads are reused, and stale values may cause memory leaks.



## what is CORS

CORS means **Cross-Origin Resource Sharing**. It is a browser security mechanism. It controls whether a web page can request data from a different origin. An origin includes the protocol, domain, and port. For example, `localhost:3000` calling `localhost:8080` is cross-origin because the port is different. The browser blocks some cross-origin requests by default for security reasons. If the backend wants to allow it, it needs to add CORS headers, such as `Access-Control-Allow-Origin`.

In Spring Boot, we can configure CORS by using `@CrossOrigin` or by defining a global CORS configuration class with `@Configuration`.

`@CrossOrigin` can be placed on a controller class or on a controller method. It means that those APIs are allowed to receive cross-origin requests from the configured origins.

- When we use `@CrossOrigin` or define a global CORS configuration, we do not need to manually add CORS response headers such as `Access-Control-Allow-Origin`.
- Spring automatically adds the appropriate CORS headers based on our configuration.



## Map vs filter

In terms of purpose, `map()` transforms each element in the stream into another element and may change its type, while `filter()` keeps or removes elements based on a condition without changing their type.

In terms of parameter types, `map()` accepts a `Function<T, R>`, while `filter()` accepts a `Predicate<T>`.



## how to send request from angular to backend

In Angular, I use `HttpClient` to send HTTP requests to the backend.

First, I provide `HttpClient` in the application and inject it into an Angular service.

Then I can use `get`, `post`, `put`, and `delete` methods to call different backend endpoints.

`HttpClient` handles HTTP requests asynchronously. It returns an `Observable`, and we usually subscribe to it to receive and process the response from the backend.

If the Angular application and backend are running on different origins, the backend must configure CORS to allow the request.

In a real project, I may also use an HTTP interceptor to add authentication tokens and handle common errors.



`HttpClient` 不会直接把最终结果返回给你，而是先返回一个 `Observable`，它代表“未来可能收到的响应”。当代码调用 `subscribe()` 时，Angular 才真正发送 HTTP 请求，并在收到响应后执行对应的处理逻辑。



## what is pattern matching

Pattern matching for `instanceof` is a new feature in Java 17.

It helps make type checking and type casting simpler. Before pattern matching, when we used the keyword `instanceof`, we had to check the type first and then manually cast the object.

With pattern matching for `instanceof`, Java checks the object type and gives us a variable of that type directly, so we do not need to manually cast it.





## Spring boot actuator

Spring Boot Actuator is a dependency that provides endpoints for monitoring  our Spring Boot application status

To use Actuator, first we need to add the Actuator dependency and then configure what endpoints are going to be exposed. 

In production, I would only expose the endpoints I need and protect sensitive endpoints 



We usually use Actuator together with Micrometer, Prometheus, and Grafana.

Spring Boot auto-configures Micrometer and collects common application metrics for us.

Prometheus periodically collects these metrics and stores them as time-series data. 

Grafana then reads the data from Prometheus and displays it on dashboards.









# Recording links for Day15 - 06/15/2026 Mock

https://xiao-java-05182026-demo-bucket.s3.us-west-2.amazonaws.com/mock-interview/2026-06-16-10-14-52.mp4?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAW3MEATYNQS2XOAZB%2F20260616%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260616T172546Z&X-Amz-Expires=604800&X-Amz-SignedHeaders=host&X-Amz-Signature=c663277a8008223b2b3031d97c7e32823558c94e9dc0d86f4354a83092c03d2f



## how to increase young generation size in heap

We can use `-XX:NewSize` to set the initial size of the young generation and `-XX:MaxNewSize` to set its maximum size.

We can also use `-Xmn` to directly set the young generation size.

`-XX:NewRatio` sets the ratio between the old generation and the young generation. A smaller value gives the young generation a larger proportion of the heap.

However, when using G1 GC, we usually do not fix the young generation size because G1 dynamically adjusts it based on the pause-time goal.



可以用 `-XX:NewSize` (dash X X colon New Size) 设置新生代初始大小，用 `-XX:MaxNewSize` 设置新生代最大大小。
 也可以用 `-Xmn` 直接指定新生代大小

`-XX:NewRatio` 用于设置老年代与新生代的比例，值越小，新生代占比越大。

使用 G1 GC 时一般不建议固定新生代大小，因为 G1 会根据停顿目标动态调整





## how do you handle exception in java

There are two main types of exceptions in Java: checked exceptions and unchecked exceptions.

For checked exceptions, such as `IOException`, the compiler forces us to handle them at compile time. We must either catch them with a `try-catch` block or declare them using the `throws` keyword.

- `try-catch` is used to catch and handle an exception. If we know that some code may fail, we can put it inside a `try` block and handle the exception in the `catch` block.
- `throws` is used in the method signature to declare that the method may pass an exception to its caller.

For unchecked exceptions, such as `RuntimeException`, the compiler does not force us to catch or declare them. They are often caused by programming errors or business rule violations.

We can define custom exceptions to make business errors more meaningful and easier to handle. I usually define custom runtime exceptions, such as `ResourceNotFoundException`. 

We use the `throw` keyword to explicitly throw a specific exception object.

In Spring Boot, I usually use `@RestControllerAdvice` and `@ExceptionHandler` for global exception handling.

- First, I create a global exception handler class and annotate it with `@RestControllerAdvice`.
- Inside that class, I use `@ExceptionHandler` on different methods to handle specific exception types. This allows the application to handle exceptions globally and return a consistent error response with the appropriate HTTP status code.
- Global exception handling avoids repeated `try-catch` blocks in every controller and keeps error handling consistent across the application.

 



## what annotations we use to configure customized actuator

If I need a custom Actuator endpoint, I can create a class and annotate it with `@Endpoint` and `@Component`. `@Endpoint` defines the endpoint ID, and `@Component` registers the class as a Spring bean.

Inside the class, I can use `@ReadOperation`, `@WriteOperation`, and `@DeleteOperation` to define read, write, and delete operations. For a web endpoint, they are usually mapped to HTTP GET, POST, and DELETE requests.

Finally, I need to expose the custom endpoint in `application.properties` by adding its endpoint ID to `management.endpoints.web.exposure.include`.





## can abstract class have no abstract method

Yes. An abstract class can have **zero abstract methods**.

An abstract class does not have to contain abstract methods, but a class containing an abstract method must be declared abstract.

 

## how can you use optional

`Optional` is a container object that may or may not contain a non-null value. We need it because it makes null handling more explicit and helps reduce `NullPointerException`.

We can create an `Optional` by using `Optional.of(value)` when the value is definitely not null, `Optional.ofNullable(value)` when the value may be null, and `Optional.empty()` when there is no value.

To use it, we can call methods like `orElse(defaultValue)`, which means if the `Optional` has a value, it returns the actual value; otherwise, it returns the default value. We can also use `orElseThrow()` to throw an exception if the value is missing, and `ifPresent()` to run some logic only when the value exists.

In practice, I usually use `Optional` as a return type when a method may not return a result



 

## What is functional interface?

A functional interface is an interface with only one abstract method.

Functional Interface is commonly used with Lambda expressions to pass behavior as an argument.

`Function`, `Consumer`, `Supplier`, and `Predicate` are common functional interfaces in Java.

They are often used in the Stream API with methods such as `filter`, `map`, and `forEach`.



## Why do you use post, instead of put

**POST** is usually used to **create a new resource** or trigger an action. POST is not idempotent.

**PUT** is usually used to **replace or update a whole resource** at a specific URL. PUT is idempotent.



## what is webflux? Have you used it in your project

Spring WebFlux is a reactive web framework used to build non-blocking and asynchronous applications.

It uses non-blocking I/O and an event-loop model, so a small number of threads can handle many concurrent I/O-bound requests.

Spring provides two main web programming models. The first is Spring MVC, which is based on the Servlet API and commonly follows the traditional thread-per-request model. The second is Spring WebFlux, which is based on Project Reactor and supports reactive, non-blocking programming.

In traditional Spring MVC, when a request thread starts a database or network I/O operation, the thread usually remains blocked until the result is returned.

In WebFlux, the thread does not wait for the I/O operation to complete. It can process other requests, and the remaining logic continues when the result becomes available.

Its core types are `Mono` and `Flux`. `Mono` represents zero or one asynchronous result, while `Flux` represents zero to many asynchronous results.

One disadvantage is that reactive code is more complex. Debugging, exception handling, and tracing can also be less intuitive than in Spring MVC.

Java 21 Virtual Threads allow developers to keep the synchronous blocking programming model while supporting high concurrency for I/O-bound workloads. They also make it easier to migrate existing Spring MVC applications.

Therefore, Virtual Threads reduce the need to choose WebFlux only for high concurrency. However, WebFlux is still a better choice when the application requires end-to-end reactive processing, back pressure, or continuous event streams.

I haven’t used it in an enterprise-level project.



## what is enable auto configuration

`@EnableAutoConfiguration` is a Spring Boot annotation.

It tells Spring Boot to automatically configure the application based on the dependencies on the classpath, existing beans, and configuration properties.

For example, if we add Spring Data JPA and a database driver, Spring Boot can automatically configure a DataSource, JPA, and transaction management.

Normally, we do not add it separately because `@SpringBootApplication` already includes `@EnableAutoConfiguration`.





# Recording links for Day14 - 06/11/2026 Mock

https://xiao-java-05182026-demo-bucket.s3.us-west-2.amazonaws.com/mock-interview/2026-06-15-09-33-42.mp4?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAW3MEATYNQS2XOAZB%2F20260615%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260615T171147Z&X-Amz-Expires=604800&X-Amz-SignedHeaders=host&X-Amz-Signature=937b1ce861c1d6174ef20008feacbca63e62755987c54daa4ccc5ea764e0b9ce



https://xiao-java-05182026-demo-bucket.s3.us-west-2.amazonaws.com/mock-interview/2026-06-15-09-57-48.mp4?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAW3MEATYNQS2XOAZB%2F20260615%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260615T171247Z&X-Amz-Expires=604800&X-Amz-SignedHeaders=host&X-Amz-Signature=1e5b74ec360dbff1c26b6c2d7943813c219d6e6fba4346af2a05d10f5565f5a8







## introduce what is Spring Framework

Spring Framework has two core features, **IoC** and **AOP**.

IoC means **Inversion of Control**. The idea is that Spring controls object creation and dependency management instead of developers doing it manually. This helps make the code loosely coupled, easier to test, and easier to maintain. 

- The main IoC container we commonly use is `ApplicationContext`. 

AOP means Aspect-Oriented Programming. It is used to add common functionality around the original business logic without changing the business code itself or introducing duplicated code everywhere.

Spring later evolved into **Spring MVC** for web application development. 

- Spring MVC is centered around the `DispatcherServlet`. The `DispatcherServlet` receives incoming HTTP requests from a servlet container such as Tomcat and uses `HandlerMapping` to find the correct controller method based on the request URL and HTTP method defined by mapping annotations such as `@RequestMapping`, `@GetMapping`, and `@PostMapping`.

After that, Spring Boot was then built on top of Spring and Spring MVC. 

- It provides auto-configuration, starter dependencies, and an embedded server such as Tomcat, so it greatly reduces manual configuration and boilerplate code.

In my projects, I mainly use Spring Boot, but the underlying concepts are still Spring Framework concepts, especially IoC, DI, and AOP.

 



## spring boot version you used

I have used both **Spring Boot 2 and Spring Boot 3**.

Spring Boot 2.x can support Java 8, 11, or 17, depending on the specific version. Spring Boot 3 requires Java 17 or later.

Another important change is the migration from Java EE APIs to Jakarta EE APIs. Spring Boot 2 commonly uses packages under the `javax` namespace, while Spring Boot 3 uses the `jakarta` namespace. 

 

## how do you define profile

In Spring Boot, I usually define profiles by creating environment-specific configuration files, such as `application-dev.properties`. We can also use YAML files, such as `application-dev.yml`.

Common environments include dev, QA, UAT, staging, and prod.

We can activate a profile by setting `spring.profiles.active` in `application.properties` or `application.yml`. 

We can also use the `@Profile` annotation on a component or configuration class. Spring only registers that bean when the specified profile is active.

Profiles help separate environment-specific configurations and allow the same codebase to run in different environments.



## What service discovery implementation have you used before?

I have used Netflix Eureka for service discovery in a Spring Cloud microservice project. 

Eureka works as a centralized service registry. When a Spring Boot service starts, it registers its service name, IP address, and port with the Eureka Server and sends heartbeats regularly.

Other microservices can discover it by its service name instead of using a hard-coded IP address. If a service instance becomes unavailable, Eureka eventually removes it from the registry.

In Spring Boot, we add the Eureka client dependency and configure the Eureka Server address. 

In modern applications, many systems run on Kubernetes or cloud-managed Kubernetes platforms.

Therefore, we often use Kubernetes Services and DNS for service discovery instead of deploying a separate Eureka Server.

Compared with Eureka, Kubernetes not only provides built-in service discovery, but also automatically restarts or replaces failed Pods.



## What is AOP

AOP stands for Aspect-Oriented Programming.

It is used to add common functionality around the original business logic without changing the business code itself or introducing duplicated code everywhere.

There are two common ways that reflect the AOP idea. One is to define an aspect class using Spring AOP. The other is to use `@RestControllerAdvice` for global exception handling.

- For a standard Spring AOP implementation, we use `@Aspect` on a class to define an aspect. We use `@Pointcut` to define where the AOP logic should be applied, and we use advice annotations to define when the logic should run.
  - Common advice annotations include `@Before`, `@After`, `@AfterReturning`, `@AfterThrowing`, and `@Around`.

- `@RestControllerAdvice` also reflects the AOP idea because it separates exception-handling logic from controller business logic. We put `@RestControllerAdvice` on a class to define a global exception handler, and we use `@ExceptionHandler` on methods inside that class to handle specific exceptions thrown by controllers.
  - This allows us to keep common exception-handling logic in one place instead of writing repeated `try-catch` blocks in multiple controllers.
  - `@RestControllerAdvice` is a Spring MVC mechanism for handling exceptions globally across multiple controllers. It reflects the AOP idea, but it is not implemented through standard Spring AOP.



## How do you write Spring Boot to call from the frontend to the backend and save data to the database

I use a three-layer architecture to handle the request flow from the frontend to the database.

The top layer is the controller layer. I use `@RestController` on a class to define a controller. Inside the class, I use annotations such as `@GetMapping` and `@PostMapping` on methods to handle different HTTP requests.

The second layer is the service layer, where I implement the business logic. We usually define a service interface and an implementation class. This gives us the flexibility to replace the implementation in the future without changing the code that uses the service.

The third layer is the repository layer, which interacts with the database. With Spring Data JPA, we just need to define a repository interface that extends an interface such as `JpaRepository`. ,Then we automatically get common CRUD methods such as `save`, `findById`, `findAll`, and `deleteById`. For additional queries, we can declare custom query methods, and Spring Data JPA can generate the implementation based on the method name.

- Spring Data JPA is a higher-level abstraction built on top of JPA. It makes data access easier and reduces boilerplate code. Under the hood, Hibernate is commonly used as the JPA implementation to perform the actual ORM operations.

  

## Describe Spring MVC

Spring MVC is a web-layer framework.

Spring MVC is centered around the `DispatcherServlet`. The `DispatcherServlet` receives incoming HTTP requests from a servlet container such as Tomcat and uses `HandlerMapping` to find the correct controller method based on the request URL and HTTP method defined by mapping annotations such as `@RequestMapping`, `@GetMapping`, and `@PostMapping`.

The controller method usually calls the service layer to process the business logic. The service layer may then call the repository layer to access the database.

After the request is processed, the result is returned to the controller. With `@RestController`, Spring automatically converts the returned object into JSON and writes it to the HTTP response body.



## how do you validate input data in spring boot

First, I define validation rules on the request DTO using annotations such as `@NotBlank`, `@Email`, and `@Size`. Then I add `@Valid` to the controller method parameter to trigger validation.

When a request comes in, Spring validates the request data before executing the business logic. If validation fails, Spring throws a validation exception.

To handle validation exceptions, I use `@RestControllerAdvice` on a class to define a global exception handler. Inside the class, I use `@ExceptionHandler` on a method to catch validation exceptions globally and return a consistent error response format.



## Spring boot actuator

Spring Boot Actuator is a dependency that provides endpoints for monitoring  our Spring Boot application status

To use Actuator, first we need to add the Actuator dependency and then configure what endpoints are going to be exposed

We use `/actuator/prometheus` to expose application metrics to Prometheus. Prometheus periodically collects these metrics and stores them as time-series data. Grafana then reads the data from Prometheus and displays it on dashboards.

In production, I would only expose the endpoints I need and protect sensitive endpoints 



## what is controller how you use controller how you implement controller

A controller is the top layer of the three-layer architecture.

The way that we are going to implement the controller is to use `@RestController` on a class to register it  as a bean. Inside the class, we use annotations such as `@GetMapping` and `@PostMapping` on methods to handle different HTTP requests.

It exposes RESTful endpoints to the client or UI, receives HTTP requests, calls the service layer, and returns HTTP responses.

The controller should mainly handle request and response logic. The service layer handles the business logic, and the repository layer interacts with the database.

In Spring, we use `@Controller` or `@RestController` on a class to define a controller.

- `@RestController` is equivalent to `@Controller` plus `@ResponseBody`. If a method in a `@RestController` returns a Java object, Spring automatically serializes it into JSON and writes it to the HTTP response body.

Inside the controller class, we use annotations such as `@GetMapping`, `@PostMapping`, `@PutMapping`, and `@DeleteMapping` to map HTTP requests to specific methods.

For controller method parameters, we can use `@RequestBody` to receive JSON data, `@PathVariable` to get values from the URL path, and `@RequestParam` to get query parameters.

So overall, the controller acts as the entry point of the backend application and connects the client with the service layer.





## what is webflux? Have you used it in your project

Spring WebFlux is a reactive web framework used to build non-blocking and asynchronous applications.

Spring provides two main web programming models. The first is Spring MVC, which is based on the Servlet API and commonly follows the traditional thread-per-request model. The second is Spring WebFlux, which is based on Project Reactor and supports reactive, non-blocking programming.

In traditional Spring MVC, when a request thread starts a database or network I/O operation, the thread usually remains blocked until the result is returned.

In WebFlux, the thread does not wait for the I/O operation to complete. It can process other requests, and the remaining logic continues when the result becomes available.

However, Java 21 introduced virtual threads as a standard feature. Virtual threads allow traditional blocking Spring MVC applications to handle a large number of concurrent requests with lower thread overhead.

Because virtual threads preserve the synchronous programming style, the code is usually easier to develop, debug, and maintain than reactive code based on `Mono` and `Flux`. Therefore, many teams no longer choose WebFlux only for the purpose of reducing thread usage. They usually choose WebFlux when the application has specific reactive or streaming requirements.



I haven’t used it in an enterprise-level project.



## how do you connect the database in springboot

To connect a database in Spring Boot, I usually follow several steps.

First, I add the required dependency, such as Spring Data JPA with the database driver, so that the application can connect to and interact with the database.

Second, I configure the database connection in `application.properties` or `application.yml`. The main configurations include the database URL, username, password, and driver. I can also configure connection pool settings, such as the maximum pool size and connection timeout.

Based on these configurations, Spring Boot automatically creates a `DataSource` and usually uses HikariCP as the default connection pool.

After the connection is established, I use the repository layer to interact with the database. The repository layer is used for database operations, while the `DataSource` configuration is responsible for establishing the connection.

 

## how do you handle global exception in spring boot

In Spring Boot, I handle global exceptions using `@RestControllerAdvice` and `@ExceptionHandler`.

First, I create a global exception handler class and annotate it with `@RestControllerAdvice`.

Inside that class, I use `@ExceptionHandler` on methods to handle specific exception types. This allows the application to catch exceptions globally and return a consistent error response with the appropriate HTTP status code.

I usually define custom runtime exceptions, such as `ResourceNotFoundException` or `BusinessException`, and throw them from the service layer when a resource cannot be found or a business rule is violated.

Global exception handling avoids repeated `try-catch` blocks in every controller and keeps error handling consistent across the application.





## Spring boot annotation

The annotations I have used in Spring Boot applications related to IoC and dependency injection, AOP, and RESTful endpoint design.

For IoC and dependency injection:

- First, I use `@SpringBootApplication` on the main class to start the Spring Boot application. It enables component scanning and auto-configuration, which help Spring discover and register beans in the IoC container.
- To register Spring beans, I use `@Controller`, `@RestController`, `@Service`, `@Repository`, and `@Component`. For third-party classes whose source code I cannot modify, I use `@Configuration` together with `@Bean`. I can also use `@Scope` to define different bean scopes.
- For dependency injection, Spring supports field injection, constructor injection, and setter injection. I mainly use constructor injection because it makes dependencies explicit, prevents missing dependencies at startup, and makes unit testing easier. When a class has only one constructor, we do not need to add `@Autowired` explicitly.

For AOP,there are two implementations that reflect the AOP idea. One is to define an aspect class using Spring AOP. The other is to use `@RestControllerAdvice` for global exception handling.

- For a standard Spring AOP implementation, we use `@Aspect` on a class to define an aspect. We use `@Pointcut` to define where the AOP logic should be applied, and we use advice annotations to define when the logic should run.
  - Common advice annotations include `@Before`, `@After`, `@AfterReturning`, `@AfterThrowing`, and `@Around`.
- `@RestControllerAdvice` also reflects the AOP idea because it separates exception-handling logic from controller business logic. We put `@RestControllerAdvice` on a class to define a global exception handler, and we use `@ExceptionHandler` on methods inside that class to handle specific exceptions thrown by controllers.
  - This allows us to keep common exception-handling logic in one place instead of writing repeated `try-catch` blocks in multiple controllers.
  - `@RestControllerAdvice` is a Spring MVC mechanism for handling exceptions globally across multiple controllers. It reflects the AOP idea, but it is not implemented through standard Spring AOP.

For RESTful endpoint design,  

- I create a controller class with `@RestController` annotation, which is equivalent to `@Controller` and `@ResponseBody`
- Then I use `@RequestMapping` to define the base URL.
- For each endpoint, I use annotations like `@GetMapping`, `@PostMapping`, `@PutMapping`, and `@DeleteMapping`. 
- I use `@PathVariable` to read values from the URL and `@RequestBody` to read JSON data from the request body. 
- For the logic inside controller methods, the controller usually calls the service layer. The service layer handles the business logic and calls the repository layer to interact with the database. I'll use @Service on the service implementation class.

 



## how Spring IOC work , all annotations and injection and bean types

Spring IoC means Inversion of Control.

Spring IoC works in several steps.

- First, when a Spring Boot application starts, `SpringApplication.run()` creates and refreshes the `ApplicationContext`, which represents the IoC container. The `ApplicationContext` is responsible for reading configuration metadata, creating and managing Spring beans, injecting their dependencies, and managing their lifecycle.
- Second, because `@SpringBootApplication` includes `@ComponentScan`, Spring scans the current package and its subpackages for classes annotated with `@Component`, `@Service`, `@Repository`, or `@Controller` and registers them as beans.
- Third, Spring creates bean instances based on these bean definitions. By default, most Spring beans are singleton, so Spring creates one shared instance of each bean within the `ApplicationContext`.
- Fourth, during bean creation, Spring finds and injects the required dependencies into each bean. This is called Dependency Injection.
  - There are 3 common ways to do dependency injection in Spring: field injection, setter injection, and constructor injection. Constructor injection is the recommended way because it makes required dependencies clear and also makes unit testing easier because we can pass mock dependencies directly when creating the object
- Finally, Spring invokes destruction callbacks when a bean’s scope ends. For example, singleton beans are destroyed when the `ApplicationContext` is closed. However, Spring does not manage the destruction of prototype beans.





## Does Spring inject dependencies by type or by name?

Spring can inject dependencies by both type and name. 

`@Autowired` mainly works by type, while `@Resource` mainly works by name. 

When multiple beans have the same type, we can use `@Qualifier` or `@Primary` to choose one.

`@Autowired` can be used on fields, constructors, and setter methods, while `@Resource` can be used on fields and setter methods, but not constructors.

For constructor injection, if the class has only one constructor, no annotation is required.

Constructor injection is the recommended way because it makes required dependencies clear and also makes unit testing easier because we can pass mock dependencies directly when creating the object



## Why do we want to use constructor injection?

First it makes required dependencies clear, 

Second, it helps prevent `NullPointerException` by ensuring that dependencies are provided when the object is created. It also supports fail-fast behavior. If a required dependency is missing, Spring reports the error during application startup or bean creation, instead of causing a `NullPointerException` later when the dependency is used.

Third, it makes unit testing easier, because we can pass mock dependencies directly when creating the object



## what java version can we use with spring boot 3

Spring Boot 3 requires Java 17 or later. If we upgrade an application from Spring Boot 2 to Spring Boot 3, we may also need to upgrade the Java version to at least Java 17.



## what is dispatcherservlet

The `DispatcherServlet` is the front controller of Spring MVC. It receives HTTP requests, uses `HandlerMapping` to find the matching handler based on the URL and HTTP method, and dispatches the request to the correct controller method.





# Recording links for **Day12 - 06/10/2026** Mock

https://xiao-java-05182026-demo-bucket.s3.us-west-2.amazonaws.com/mock-interview/2026-06-10-23-26-23.mp4?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAW3MEATYNQS2XOAZB%2F20260611%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260611T063031Z&X-Amz-Expires=604800&X-Amz-SignedHeaders=host&X-Amz-Signature=a040da53d049cf37b5e9bb8fec952fbd63d0a84f3a5a8157d67f4ff9a50be59f



## how to use stream to filter people younger than 30

First, we use `.stream()` to convert the list into a stream.

Then, we use the `.filter()` method with a predicate such as `person -> person.getAge() < 30`.

Finally, we use a terminal operation like `.collect(Collectors.toList())` to convert the stream into a list.



```java
List<Person> result = people.stream()
        .filter(person -> person.getAge() < 30)
        .toList();
```



## where did you use singleton in your project

In my project, I used singleton mainly for shared resources that should have only one instance, such as thread pools, because creating these resources repeatedly would be expensive and could hurt performance.

Also, in Spring Boot, most service and repository beans are singleton-scoped by default, which means Spring’s dependency injection reuses the same bean instance across the application.

For infrastructure resources, such as database connection pools, cache clients, message queue clients, I usually let Spring manage them through `@Bean` or auto-configuration.

- Business classes get these shared instances through dependency injection.
- This is better than manually implementing a singleton and calling `Singleton.getInstance()`, because it is easier to test, replace, configure for different environments, and manage the lifecycle.
- It is also more consistent with the Spring Boot style.



PostgreSQL 项目里，我们通常面向 `DataSource` 编程，Spring Boot 默认用 HikariCP 作为底层连接池。业务代码不需要自己 new connection，也不需要自己写 singleton。

在 Spring Boot 里，只要 classpath 上有 JDBC/JPA，并且配置了 `spring.datasource.url`、`username`、`password` 这类 datasource 配置，Spring Boot 会自动创建一个 `DataSource` Bean。默认优先使用 **HikariCP**，也就是 `HikariDataSource`。

在 Spring Boot + PostgreSQL 里，如果你没有手动配置：

```
spring.datasource.hikari.maximum-pool-size=...
spring.datasource.hikari.minimum-idle=...
```

那么 Spring Boot 自动创建 `HikariDataSource` 后，会使用 HikariCP 自己的默认值。

通常默认是：

```
maximumPoolSize = 10
minimumIdle = same as maximumPoolSize，也就是 10
```



只要项目里有 JDBC/JPA 相关依赖，并且配置了 database 相关信息，比如 `spring.datasource.url`、`username`、`password`，Spring Boot 就会自动创建一个 `DataSource` bean。

默认情况下，Spring Boot 通常会使用 HikariCP 作为底层连接池实现，也就是创建一个 `HikariDataSource`。如果我们没有手动配置连接池大小，HikariCP 默认的 maximum pool size 通常是 10。

所以在 Spring Boot 项目里，我们一般不需要自己手写 Singleton pattern 来管理数据库连接池。我们只需要让 Spring 管理 `DataSource` bean，然后业务代码通过依赖注入使用它。



注册成 Spring bean



基础设施资源，比如数据库连接池、缓存客户端、消息队列客户端、线程池，通常交给 Spring `@Bean` 或自动配置来管理。业务类通过依赖注入拿到这些共享实例。

这种方式比自己手写单例模式、然后调用 `Singleton.getInstance()` 更容易测试、替换实现、按不同环境做配置，也更方便让 Spring 管理对象的生命周期，所以更符合 Spring Boot 项目的习惯。



Spring Data Redis 里通常有一个核心组件：`RedisConnectionFactory`，它负责创建和管理 Redis 连接。底层可能用 Lettuce 或 Jedis。然后 Spring 会基于它创建：`RedisTemplate` 和 `StringRedisTemplate`

业务代码一般注入 `StringRedisTemplate` 或 `RedisTemplate` 来操作 Redis。

Spring Kafka 会帮你配置：

```
ProducerFactory
ConsumerFactory
KafkaTemplate
KafkaListenerContainerFactory
```

其中：

`ProducerFactory` 负责创建 Kafka producer。
 `ConsumerFactory` 负责创建 Kafka consumer。
 `KafkaTemplate` 是业务代码发送消息时常用的入口。
 `@KafkaListener` 背后由 listener container 管理消费者线程和消费逻辑。

所以业务代码通常只是注入：`KafkaTemplate`，它是**Spring Kafka 提供的一个工具类**，主要用来在 Spring Boot 项目里**发送消息到 Kafka**。







## what is SOLID principle

SOLID is a set of five principles for object-oriented design.

Single Responsibility means a class should focus on one responsibility.

Open-Closed means the code should be open for extension but closed for modification.

Liskov (/ˈlɪs.kɒv/) Substitution means a child class should be able to replace its parent class safely.

Interface Segregation means we should use small and focused interfaces instead of one huge interface.

Dependency Inversion means high-level modules should depend on abstractions instead of concrete classes.

- In Spring Boot, Dependency Inversion is commonly achieved through dependency injection.e.





# Recording links for **Day11 - 06/09/2026** Mock

https://xiao-java-05182026-demo-bucket.s3.us-west-2.amazonaws.com/mock-interview/2026-06-10-11-50-41.mp4?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAW3MEATYNQS2XOAZB%2F20260610%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260610T190052Z&X-Amz-Expires=604800&X-Amz-SignedHeaders=host&X-Amz-Signature=5d5d2f9daf7cfcd3ee280cfb6c9541d429f8af9e0d2fccfa6fab40f9d7c4f0ef



https://xiao-java-05182026-demo-bucket.s3.us-west-2.amazonaws.com/mock-interview/2026-06-10-12-23-27.mp4?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAW3MEATYNQS2XOAZB%2F20260610%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260610T192757Z&X-Amz-Expires=604800&X-Amz-SignedHeaders=host&X-Amz-Signature=cf77a5dc5ed406dc7243b2abf0a7413c5c9da3b919d9d282e181f42e16cd376b



## how to group people to key(age) value (list of people)

If we are given a list of people, there are two ways to group them by age.

One way is to create a map manually and use a `for` loop to iterate over the list and populate the map.

Another way that is more cleaner is to use the Stream API to group people by age.

First, I call `.stream()` to convert the list into a stream. Then I use `.collect()` with `Collectors.groupingBy(Person::getAge)` — Collectors dot grouping by, Person double colon get age. This returns a `Map<Integer, List<Person>>`.

The key is an `Integer` that represents the age, and the value is a list of people in that age group.



## Recursion vs Iteration

**Recursion** means a function keeps calling itself to solve a smaller version of the same problem  until it reaches a **base case**.

**Iteration** means using a for loop or a while loop to repeat logic.

In terms of memory, 

- **recursion** uses the **call stack**. If recursion is too deep, it may cause **StackOverflowError**.

- **Iteration** is usually more memory-efficient, so it is often preferred in production for simple repeated logic.

In terms of readability, 

- recursion can be cleaner for problems like tree traversal, DFS, and backtracking. 
- For those problems, it can be harder to write the code using iteration.



## how to write rest api in springboot

To write REST APIs in Spring Boot, I create a controller class with `@RestController`. annotation

Then I use `@RequestMapping` to define the base URL.

 For each endpoint, I use annotations like `@GetMapping`, `@PostMapping`, `@PutMapping`, and `@DeleteMapping`. 

I use `@PathVariable` to read values from the URL and `@RequestBody` to read JSON data from the request body. 

For the logic inside controller methods, the controller usually calls the service layer. The service layer handles the business logic and calls the repository layer to interact with the database.

For responses, if the response is simple, I can just return a DTO, and Spring Boot will automatically convert it to JSON and write it to the HTTP response. If I need more control, I can use `ResponseEntity` as the return type because it allows me to set the HTTP status code, response headers, and response body.



## linked hashmap vs hashmap

They are both implementations of the `Map` interface, and both store key-value pairs.

In terms of order,

-  `HashMap` does not guarantee any iteration order.
-  `LinkedHashMap` maintains insertion order.



In terms of internal structure, 

- `HashMap` is based on a hash table. It handles hash collisions by using a linked list, and when there are too many collisions in one bucket, the linked list can be converted into a red-black tree.
- `LinkedHashMap` is also based on a hash table, but it adds a doubly linked list to maintain order.

In terms of performance, 

- both `HashMap` and `LinkedHashMap` usually have O(1) average time complexity for `put`, `get`, and `remove`.
- But `LinkedHashMap` has a little extra memory cost because it maintains the doubly linked list.



## Java 11 new features

Java 11 removed some Java EE and CORBA (/ˈkɔːr.bə/) modules from the JDK, so old applications may need to add separate dependencies. 

- Java EE was later renamed to Jakarta EE. Starting from Jakarta EE 9, the package names changed from javax.* to jakarta.*. So if we use newer frameworks based on Jakarta EE 9 or later, such as Spring Boot 3 and Tomcat 10, we should use jakarta.* package names. 

Java 11 also introduced the standard **HTTP Client API**.  It solves the problem that older Java HTTP APIs, like `HttpURLConnection`, were verbose and hard to use. The new HTTP Client provides a cleaner way to send HTTP requests, and it supports synchronous calls, asynchronous calls, and HTTP/2 (HTTP two).

Java 11 also introduced some new garbage collectors, such as ZGC and Epsilon GC. ZGC focuses on low-latency garbage collection, and Epsilon is a no-op garbage collector mainly used for testing and performance experiments.



## design the locking schema

design the locking schema so that when a thread call method1(), it needs to wait until some other thread call method2()

We can use `synchronized`, `wait()`, and `notifyAll()` to coordinate two methods. We also use a boolean flag called `method2Called`, which is initialized to false

In `method1()`, the thread enters the synchronized block and checks the flag. If `method2Called` is false, it calls `lock.wait()` and releases the lock. The thread will stay waiting until another thread wakes it up.

In `method2()`, the thread enters the same synchronized block, sets `method2Called` to true, and calls `lock.notifyAll()` to wake up the waiting thread.

After `method1()` wakes up, it checks the condition again in the `while` loop. If the flag is true, it continues executing the rest of the logic.

```Java
public class LockingExample {

    private final Object lock = new Object();
    private boolean method2Called = false;

    public void method1() throws InterruptedException {
        synchronized (lock) {
            while (!method2Called) {
                lock.wait();
            }

            // method1 logic after method2 is called
            System.out.println("method1 continues");
        }
    }

    public void method2() {
        synchronized (lock) {
            method2Called = true;
            lock.notifyAll();
        }
    }
}
```



# Recording links for **Day10 - 06/08/2026** Mock

https://xiao-java-05182026-demo-bucket.s3.us-west-2.amazonaws.com/mock-interview/2026-06-09-11-02-33.mp4?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAW3MEATYNQS2XOAZB%2F20260609%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260609T180546Z&X-Amz-Expires=604800&X-Amz-SignedHeaders=host&X-Amz-Signature=a6ece6a4ec2e915c74d9d2769575192a47df1e1dd9585f1f1671f6165d589322

**what is pattern matching**

Pattern matching for `instanceof` is a new feature in Java 17.

It helps make type checking and type casting simpler. Before pattern matching, when we used the keyword `instanceof`, we had to check the type first and then manually cast the object.

With pattern matching for `instanceof`, Java checks the object type and gives us a variable of that type directly, so we do not need to manually cast it.





Recording links for **Day9 - 06/05/2026** Mock

https://xiao-java-05182026-demo-bucket.s3.us-west-2.amazonaws.com/mock-interview/2026-06-07-12-37-20.mp4?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAW3MEATYNQS2XOAZB%2F20260607%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260607T195339Z&X-Amz-Expires=604800&X-Amz-SignedHeaders=host&X-Amz-Signature=e6fad5b428bf76eb362826630c7d8aa712422be7de7930696900e294fde2a366





Recording link for **Day5 - 06/01/2026** Mock

https://xiao-java-05182026-demo-bucket.s3.us-west-2.amazonaws.com/mock-interview/2026-06-01-23-29-01.mp4?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAW3MEATYNQS2XOAZB%2F20260602%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260602T065208Z&X-Amz-Expires=604800&X-Amz-SignedHeaders=host&X-Amz-Signature=8e654ad0d39234a6f3efd187abb618f5144e010d6ec028dacbf557b34663f411



Recording links for **Day4 - 05/29/2026** Mock

https://xiao-java-05182026-demo-bucket.s3.us-west-2.amazonaws.com/mock-interview/2026-05-31-22-02-12.mp4?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAW3MEATYNQS2XOAZB%2F20260601%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260601T055723Z&X-Amz-Expires=604800&X-Amz-SignedHeaders=host&X-Amz-Signature=3a2332c7bd69404e0a82fa43060b15a4bd80cfcfb57be70c0ca01de676a668ec



https://xiao-java-05182026-demo-bucket.s3.us-west-2.amazonaws.com/mock-interview/2026-05-31-22-26-05.mp4?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAW3MEATYNQS2XOAZB%2F20260601%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260601T055820Z&X-Amz-Expires=604800&X-Amz-SignedHeaders=host&X-Amz-Signature=b1fa8cd3bf9e96867521b64d7b5b75b2ebbe06addb2322df5ef3bcbd3840bf44

