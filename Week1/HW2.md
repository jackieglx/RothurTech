

# String vs StringBuilder vs StringBuffer

String:
- Immutable.
- Every modification creates a new object.
- Good for fixed text or small/simple string operations.

StringBuilder:
- Mutable.
- Not thread-safe.
- Faster than StringBuffer.
- Good for frequent string modifications in single-thread scenarios.

StringBuffer:
- Mutable.
- Thread-safe because methods are synchronized.
- Slower than StringBuilder.
- Used when multiple threads modify the same string object.

# Comparator vs Comparable

Comparable:
- Defines natural sorting inside the class.
- Class implements Comparable<T>.
- Method: compareTo()

```java
class Student implements Comparable<Student> {
    public int compareTo(Student other) {
        return this.age - other.age;
    }
}
```

Comparator:
- Defines sorting rules outside the class.

```java
Comparator<Student> byName = (a, b) -> a.name.compareTo(b.name);
```

Summary:
- Use Comparable when the class has one natural order.
- Use Comparator when you need external or multiple sorting rules.



# Overriding vs Overloading

Overloading:
- Same method name, different parameter list.
- Happens in the same class or child class.
- Compile-time polymorphism.

Example:
void add(int a, int b)
void add(double a, double b)

Overriding:
- Child class provides a new implementation of parent class method.
- Method name, parameters, and return type should be compatible.
- Runtime polymorphism.

Example:

```java
class Parent {
    void run() {}
}

class Child extends Parent {
    @Override
    void run() {}
}
```



# JRE vs JDK vs JVM

JVM:
- Java Virtual Machine.
- Runs Java bytecode.
- Provides memory management, class loading, and GC.

JRE:
- Java Runtime Environment.
- Contains JVM + core libraries.
- Used to run Java programs.

JDK:
- Java Development Kit.
- Contains JRE + development tools.
- Used to develop, compile, and run Java programs.


JDK > JRE > JVM



# Java 8 primitive data types

Java has 8 primitive types:

byte    - 1 byte
short   - 2 bytes
int     - 4 bytes
long    - 8 bytes
float   - 4 bytes
double  - 8 bytes
char    - 2 bytes
boolean - true / false



# Primitive type vs Reference type

Primitive type:
- Stores actual value.
- Examples: int, long, double, boolean.
- Usually stored in stack when used as local variables.

Reference type:
- Stores reference/address to an object.
- Examples: String, Object, ArrayList, arrays.
- Object data is stored in heap.



# How does JVM work

Basic process:

1. Java source code is compiled into bytecode: `.java -> .class`
   
2. ClassLoader loads class files into JVM.

3. Bytecode Verifier checks bytecode safety.

4. Execution Engine runs bytecode:
   - Interpreter executes line by line.
   - JIT compiler compiles hot code into native machine code.

5. Runtime Data Areas manage memory.

6. GC automatically reclaims unused objects.



# JVM memory data model

Main JVM runtime memory areas:

Program Counter Register:
- Thread-private.
- Stores the address of the current instruction.

Java Virtual Machine Stack:
- Thread-private.
- Stores stack frames.
- Each method call creates one stack frame.
- Contains local variables, operand stack, return address.

Native Method Stack:
- Thread-private.
- Used for native methods.

Heap:
- Thread-shared.
- Stores objects and arrays.
- Main area managed by GC.

Method Area / Metaspace:
- Thread-shared.
- Stores class metadata, method metadata, runtime constant pool.
- In Java 8, PermGen was replaced by Metaspace.



# How does GC work

GC is used to automatically reclaim memory from objects that are no longer reachable.

Basic steps:

1. Find live objects:
   - Start from GC Roots.
   - Follow references to find reachable objects.

2. Objects not reachable are considered garbage.

3. Reclaim memory:
   - Remove unreachable objects.
   - Compact memory if needed.

Common GC Roots:
- Local variables in stack.
- Static variables.
- Active threads.
- JNI references.



# Young / Old / Perm Generation

Young Generation:
- Stores newly created objects.
- Most objects die here.
- Divided into Eden, Survivor 0, Survivor 1.
- Minor GC happens here.

Old Generation:
- Stores long-lived objects.
- Objects surviving many Minor GCs may be promoted here.
- Major GC / Full GC may happen here.

Perm Generation:
- Used before Java 8.
- Stored class metadata.
- Replaced by Metaspace in Java 8.



# Different types of GC

Minor GC:
- Happens in Young Generation.
- Usually fast and frequent.

Major GC:
- Happens in Old Generation.
- Usually slower than Minor GC.

Full GC:
- Cleans the entire heap and possibly Metaspace.
- Usually expensive and should be minimized.

Common GC collectors:

Serial GC:
- Single-threaded.
- Suitable for small applications.

Parallel GC:
- Multi-threaded.
- Focuses on high throughput.

CMS GC:
- Concurrent Mark Sweep.
- Reduces pause time.
- Mostly replaced by G1.

G1 GC:
- Garbage First.
- Divides heap into regions.
- Balances throughput and low pause time.
- Default GC in many modern JDK versions.

ZGC / Shenandoah:
- Low-latency collectors.
- Designed for very large heaps and very short pause times.