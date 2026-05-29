# Java Modifier Scope: public, private, protected, default

public:
- Accessible from anywhere.
- Use for APIs or methods/classes that must be exposed.

private:
- Accessible only inside the same class.
- Use for internal fields and helper methods.
- Best for encapsulation.

protected:
- Accessible in the same package and by subclasses.
- Use when child classes need access.

default / package-private:
- No modifier; accessible only within the same package.
- Use for internal package-level classes or methods.

# What is static scope?

- static members belong to the class, not to an object.
- Access by class name: ClassName.method() or ClassName.field.
- static data is shared by all objects of the class.
- Use for utility methods, constants, and shared class-level data.

# How does ClassLoader work?

- ClassLoader loads .class files into JVM memory.
- Main process: Loading -> Linking -> Initialization.
- Loading finds and loads class bytecode.
- Linking verifies bytecode, prepares static fields, and resolves references.
- Initialization executes static variables and static blocks.
- Java uses parent delegation: ask parent ClassLoader first, then load by itself.

# Checked Exception vs Unchecked Exception

Checked Exception:
- Checked at compile time.
- Must use try-catch or throws.
- Examples: IOException, SQLException.
- Usually external problems, like file or database errors.

Unchecked Exception:
- Checked at runtime.
- Not required to catch or declare.
- Examples: NullPointerException, ArithmeticException.
- Usually programming bugs.

# finally vs final vs finalize

final:
- A keyword.
- Can modify class, method, or variable.
- Class cannot be inherited, method cannot be overridden, variable cannot be reassigned.

finally:
- A block used with try-catch.
- Executes cleanup logic.
- Usually runs whether exception happens or not.

finalize:
- A method called by GC before object destruction.
- Was used for cleanup in old Java.
- Not recommended; deprecated because it is unreliable.

# try-with-resources

- A try statement that automatically closes resources.
- Resource must implement AutoCloseable or Closeable.
- Ordinary try needs manual close; try-with-resources closes automatically.
- Use for I/O, database connections, streams, sockets.

# RuntimeException

- RuntimeException is the parent class of unchecked exceptions.

- It usually happens because of programming mistakes.

- It does not require mandatory try-catch.

  

# NoClassDefFoundError vs ClassNotFoundException

ClassNotFoundException:
- A checked exception.
- Usually caused by Class.forName() or reflection when class is not found.
- Example: Class.forName("com.demo.User")

NoClassDefFoundError:
- An Error.
- Class existed at compile time but is missing at runtime.
- Example: missing jar file during runtime.

Key difference:
- ClassNotFoundException is an exception you can handle.
- NoClassDefFoundError is usually a classpath/runtime environment problem.

# Why clean up I/O resources in finally?

- I/O resources use external system resources, such as files, sockets, or database connections.
- If not closed, they may cause resource leaks.
- Close them in finally or use try-with-resources.
- Prefer try-with-resources in modern Java.

# OutOfMemoryError

- OutOfMemoryError means JVM cannot allocate enough memory.
- It is an Error, not a normal Exception.
- Common causes: memory leak, too many objects, large arrays, too small heap.
- Solutions: check memory leaks, optimize object usage, tune JVM heap size.

# Generics in Java

- Generics allow classes, interfaces, and methods to use type parameters.
- It avoids unsafe casts and improves type safety.
- Advantages: compile-time type checking, less casting, more reusable code.

# How Generics work: Type Erasure

- Java Generics are implemented by type erasure.
- Generic type information is checked at compile time and mostly removed at runtime.
- This keeps compatibility with older Java versions.
- Generic types are mainly a compile-time safety feature.

# List<? extends T> vs List<? super T>

List<? extends T>:
- Means T or subclass of T.
- Used to read data from the list.
- Can read as T, but cannot safely add elements except null.
- Also called Producer.
- Example: List<? extends Number>

List<? super T>:
- Means T or parent class of T.
- Used to write data into the list.
- Can add T or subclasses of T.
- Also called Consumer.
- Example: List<? super Integer>

Memory tip:
- PECS: Producer Extends, Consumer Super.

# Optional Class

- Optional is a container that may or may not contain a value.
- It helps reduce null checks and NullPointerException.
- Use as a return type when a value may be absent.
- Do not overuse it for fields or method parameters.

Demo:

```java
  String name = null;

  Optional<String> optional = Optional.ofNullable(name);

  String value1 = optional.orElse("default name");

  String value2 = optional.orElseThrow(
      () -> new RuntimeException("name is required")
  );
```



# OOP

- OOP means Object-Oriented Programming.
- Core idea: model software using objects that contain data and behavior.

Encapsulation:
- Hide internal data and expose methods.
- Protect object state.

Inheritance:
- Child class reuses parent class features.
- Used for code reuse and extension.

Polymorphism:
- Same method call can have different behaviors.
- Makes code flexible and extensible.

Abstraction:
- Hide implementation details and expose essential behavior.
- Usually implemented by interfaces and abstract classes.