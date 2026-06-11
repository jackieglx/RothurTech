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

