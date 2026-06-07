# write out the optimized Singleton Version and explain each line of code

```java
public enum Singleton {
    INSTANCE;

    public void doSomething() {
        System.out.println("Doing something...");
    }
}
```

`public enum Singleton` defines a special Java enum type. `INSTANCE` is the only object of this enum, so it represents the single Singleton instance. Java guarantees that enum constants are initialized only once in a thread-safe way, and it also handles serialization safely. Therefore, enum singleton is simple, safe, and usually the recommended Singleton implementation in Java.

Enum singleton is thread-safe in terms of instance creation. Even if multiple threads access `Singleton.INSTANCE` at the same time, the JVM ensures only one instance is created and safely published. If the singleton has mutable fields or non-thread-safe methods, we still need to handle synchronization separately.

# what is reflection

Reflection is a feature that allows a program to inspect and modify classes, methods, fields, and constructors at runtime.

Reflection allows us to work with objects even when we do not know the exact class definition at compile time. This is very important for framework design and generic tools.

A very common example is Spring Boot dependency injection. Spring Boot can scan the classpath, find classes with annotations like `@Service` and `@RestController`, and then create their objects automatically.

But reflection also has some drawbacks. It can be slower than normal method calls, it can break encapsulation by accessing private fields or methods, and it is harder to debug.



# http status code

what are http status code, 200/ 201/202/ 204/ 307/ 301/ 400/ 401/ 403/ 404/ 500, explain them by your own words



HTTP status codes are numbers returned by the server to tell the client the result of an HTTP request.

Two-hundred-level status codes mean success, 

three-hundred-level means redirection, 

four-hundred-level means client error, 

and five-hundred-level means server error.



For these common ones:

`200 OK` means a successful `GET` request.

`201 Created`  commonly means a successful `POST` request.

`202 Accepted` means the request has been accepted, but it has not been completed yet. For example, an async task is submitted.

`204 No Content` means the request succeeded, but the response body is empty. It is common for a successful `DELETE` or update operation.

`301 Moved Permanently` means the resource has been permanently moved to a new URL.

`307 Temporary Redirect` means the resource is temporarily redirected to another URL, and the client should keep the same HTTP method.

`400 Bad Request` means the client sent an invalid request, such as invalid parameters or invalid JSON.

`401 Unauthorized` means authentication is missing or invalid. For example, the user does not provide a valid token.

`403 Forbidden` means the user is authenticated, but does not have permission to access the resource.

`404 Not Found` means the requested resource does not exist.

`500 Internal Server Error` means something went wrong on the server side.



# what is http 

HTTP stands for **HyperText Transfer Protocol**.

It is the main protocol used by browsers and servers to communicate on the web.

HTTP is based on a **request-response model**, which means the server can only send a response after receiving a request from the client. The server cannot actively push data to the client over traditional HTTP. If the server needs to send data to the client proactively, it needs to use WebSocket.

**HTTP itself is not encrypted**. When HTTP is used with TLS encryption, it becomes **HTTPS**.



# what is get, post, put, delete, patch method

They are common **HTTP methods**. They tell the server what kind of operation the client wants to perform.

`GET` is used to read data from the server. It should not change data. For example, getting a student by ID.

`POST` is usually used to create a new resource. For example, creating a new student.

`PUT` is usually used to update or replace a whole resource. For example, updating the full student profile.

`PATCH` is used to partially update a resource. For example, only updating a student's email.

`DELETE` is used to delete a resource. For example, deleting a student by ID.



# post vs patch

`POST` is usually used to **create a new resource** or trigger a server-side action.

`PATCH` is used to **partially update an existing resource**.

# post vs put

`POST` is usually used to **create a new resource**.

`PUT` is usually used to **replace or fully update** an existing resource.



# What is idempotent, which http method is idempotent?

Idempotent means if we send the same request multiple times, the final result on the server should be the same as sending it once.

For HTTP methods:

GET, DELETE and PUT are idempotent.

`GET` is idempotent because reading the same data multiple times should not change the server data.

`DELETE` is also idempotent because deleting the same resource multiple times has the same final result: the resource is gone.

`PUT` is idempotent because updating the same resource with the same data multiple times should leave the resource in the same final state.

`POST` is usually not idempotent because sending the same `POST` request multiple times may create multiple resources.

`PATCH` is not always idempotent. It depends on how the update is designed.