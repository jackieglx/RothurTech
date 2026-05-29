1. What is functional interface?

A functional interface is an interface that has **only one abstract method**.
 It can be used with lambda expressions, such as `Runnable`, `Predicate`, `Function`, and `Consumer`.

------

2. What is default method?

A default method is a method in an interface that has a method body, using the `default` keyword.
 It allows interfaces to add new methods without breaking existing implementation classes.

------

3. What is the difference between Predicate, Supplier, Consumer, Function?

`Predicate<T>` takes one input and returns `boolean`; `Supplier<T>` takes no input and returns a value.
 `Consumer<T>` takes one input and returns nothing; `Function<T, R>` takes one input and returns another value.

------

4. Code: use Predicate, Supplier, Consumer, Function

```java
import java.util.function.*;

public class FunctionalInterfaceDemo {
    public static void main(String[] args) {
        Predicate<Integer> isAdult = age -> age >= 18;
        System.out.println(isAdult.test(20)); // true

        Supplier<String> supplier = () -> "Hello Java";
        System.out.println(supplier.get());

        Consumer<String> printer = name -> System.out.println("Name: " + name);
        printer.accept("Alice");

        Function<String, Integer> getLength = str -> str.length();
        System.out.println(getLength.apply("Java")); // 4
    }
}
```

------

5. What is method reference?

A method reference is a shorter way to write a lambda expression when the lambda only calls an existing method.
 For example, `System.out::println` is the method reference version of `x -> System.out.println(x)`.

------

6. What is CompletableFuture?

`CompletableFuture` is used for asynchronous programming in Java.
 It allows a task to run in the background and then chain the next actions using methods like `thenApply()`, `thenAccept()`, and `exceptionally()`.

------

7. Default keyword vs Java default scope

The `default` keyword is used in an interface to define a method with a method body.
 Java default scope means no access modifier is written, so the class, method, or field is only accessible within the same package.

------

8. Coding: Student Stream API

```java
import java.util.*;
import java.util.function.Function;
import java.util.stream.Collectors;

class Student {
    private String name;
    private int age;
    private int score;
    private String gender;

    public Student(String name, int age, int score, String gender) {
        this.name = name;
        this.age = age;
        this.score = score;
        this.gender = gender;
    }

    public String getName() {
        return name;
    }

    public int getAge() {
        return age;
    }

    public int getScore() {
        return score;
    }

    public String getGender() {
        return gender;
    }

    @Override
    public String toString() {
        return "Student{name='" + name + "', age=" + age + ", score=" + score + ", gender='" + gender + "'}";
    }
}

public class StudentStreamDemo {
    public static void main(String[] args) {
        List<Student> list = new ArrayList<>();

        list.add(new Student("Alice", 18, 95, "girl"));
        list.add(new Student("Amy", 19, 88, "girl"));
        list.add(new Student("Bob", 20, 55, "boy"));
        list.add(new Student("Alex", 18, 70, "boy"));
        list.add(new Student("Cindy", 19, 60, "girl"));

        // 1. Find all students' names starting with 'A'
        List<String> namesStartingWithA = list.stream()
                .map(Student::getName)
                .filter(name -> name.startsWith("A"))
                .collect(Collectors.toList());

        System.out.println(namesStartingWithA);

        // 2. Get the sum of all students' scores
        int totalScore = list.stream()
                .mapToInt(Student::getScore)
                .sum();

        System.out.println(totalScore);

        // 3. Find all students whose score >= 60
        List<Student> passedStudents = list.stream()
                .filter(student -> student.getScore() >= 60)
                .collect(Collectors.toList());

        System.out.println(passedStudents);

        // 4. Retrieve all students' names
        List<String> allNames = list.stream()
                .map(Student::getName)
                .collect(Collectors.toList());

        System.out.println(allNames);

        // 5. Count the frequency of each age
        Map<Integer, Long> ageFrequency = list.stream()
                .collect(Collectors.groupingBy(Student::getAge, Collectors.counting()));

        System.out.println(ageFrequency);

        // 6. Count the number of boys and girls using groupingBy
        Map<String, Long> genderCountByGrouping = list.stream()
                .collect(Collectors.groupingBy(Student::getGender, Collectors.counting()));

        System.out.println(genderCountByGrouping);

        // 7. Count the number of boys and girls using Collectors.toMap()
        Map<String, Long> genderCountByToMap = list.stream()
                .collect(Collectors.toMap(
                        Student::getGender,
                        student -> 1L,
                        Long::sum
                ));

        System.out.println(genderCountByToMap);
    }
}
```

------

9. Intermediate operation vs terminal operation

Intermediate operations return another stream and are lazy, so they do not run immediately, such as `filter()`, `map()`, and `sorted()`.
 Terminal operations trigger the stream execution and return the final result, such as `collect()`, `forEach()`, `count()`, and `sum()`.

------

10. Coding: given a char array, use Stream API to count frequency of each char

```java
import java.util.*;
import java.util.function.Function;
import java.util.stream.Collectors;
import java.util.stream.IntStream;

public class CharFrequencyDemo {
    public static void main(String[] args) {
        char[] chars = {'a', 'b', 'a', 'c', 'b', 'a'};

        Map<Character, Long> charFrequency = IntStream.range(0, chars.length)
                .mapToObj(i -> chars[i])
                .collect(Collectors.groupingBy(Function.identity(), Collectors.counting()));

        System.out.println(charFrequency);
    }
}
```

11. Stream API: map() vs flatMap()

`map()` transforms each element into one new element, so it keeps the stream structure one-to-one.
 `flatMap()` transforms each element into a stream and then flattens all small streams into one single stream.