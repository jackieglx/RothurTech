# Recording links for **Day11 - 06/09/2026** Mock

https://xiao-java-05182026-demo-bucket.s3.us-west-2.amazonaws.com/mock-interview/2026-06-10-11-50-41.mp4?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAW3MEATYNQS2XOAZB%2F20260610%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260610T190052Z&X-Amz-Expires=604800&X-Amz-SignedHeaders=host&X-Amz-Signature=5d5d2f9daf7cfcd3ee280cfb6c9541d429f8af9e0d2fccfa6fab40f9d7c4f0ef



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



Recording links for **Day10 - 06/08/2026** Mock

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

