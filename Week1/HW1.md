# Homework1

Question List (write necessary code to answer the following questions)

1. List vs Set

   - A `List` stores elements in insertion order.
   - A `List` allows duplicate elements.
   - Common implementations of `List` include `ArrayList` and `LinkedList`.
   - A `Set` stores unique elements.
   - Common implementations of `Set` include `HashSet`, `TreeSet`, and `LinkedHashSet`.

2. LinkedList vs ArrayList

   - `ArrayList` is backed by a dynamic array.
     - It is fast to access elements by index, because `get(index)` is usually `O(1)`.
     - But inserting or removing elements in the middle can be slow, because other elements may need to be shifted.
   - `LinkedList` is backed by a doubly linked list.
     - It is slower to access elements by index, because it may need to traverse the list.
     - But adding or removing elements at the beginning or end can be efficient.

3. What is Map Interface

   - `Map` is an interface used to store key-value pairs. Keys cannot be duplicated, but values can be duplicated.
   - Common implementations include `HashMap`, `Hashtable`, `ConcurrentHashMap`, `LinkedHashMap`, and `TreeMap`.

4. How does HashMap work

   - **Write / put**

     -  When we call `put(key, value)`, HashMap first uses the key’s `hashCode()` to calculate a hash value. 
     -  Then it uses the hash value to find the bucket, which is a position in the internal array. 
     -  If the bucket is empty, it directly stores the key-value pair there. 
     -  If the bucket already has elements, there is a hash collision. HashMap uses `equals()` to check whether the key already exists. 
     -  If the key exists, it updates the value. If not, it adds a new node to the linked list or red-black tree. 
     -  When the number of entries is greater than the capacity times the load factor, HashMap resizes the array and redistributes the existing entries. 

     **Read / get**

     -  When we call `get(key)`, HashMap calculates the bucket position in the same way. 
     -  Then it searches in that bucket using the hash value and `equals()`. 
     -  If it finds the key, it returns the corresponding value.

5. What is hash collision

   - Hash collision means two different keys get the same hash value or are placed in the same bucket.
   - In `HashMap`, Java first uses the key’s `hashCode()` to calculate where the key should be stored.
   - But different keys may be mapped to the same position.
   - When this happens, `HashMap` cannot only use the hash value.
   - It also uses `equals()` to check whether the keys are really the same.
   - If one bucket has multiple entries, Java stores them in a linked list or a red-black tree.

6. What is Collections used for

   -  Collections is a utility class. It provides many static methods to work with collections. For example, `Collections.sort(list)`, `Collections.reverse(list)`, and `Collections.max(list)`.

7. What is immutable class

   - An immutable class means once an object is created, its state cannot be changed.
   - That means after the field values are set, we cannot change them through methods.
   - Usually, we make the class `final`, make fields `private final`, and do not provide setter methods.

8. HashTable vs HashMap vs ConcurrentHashmap

   - `Hashtable` is a **legacy thread-safe implementation** of `Map`.
     -  It is backed by a hash table. 
     -  Its basic operations are usually `O(1)`. 
     -  It uses synchronized methods, so **most operations lock the whole table**, which can hurt performance in multi-threaded environments. 
     -  It does not allow `null` keys or `null` values. 
     -  In modern Java, `ConcurrentHashMap` is usually preferred over `Hashtable`.

   - `HashMap` stores key-value pairs with no guaranteed order. 
     -  It is backed by a **hash table**. 
     -  Its basic operations like `put`, `get`, and `remove` are usually `O(1)`. 
     -  It allows one `null` key and multiple `null` values. 
     -  It is not thread-safe. 
   - `ConcurrentHashMap` is a thread-safe implementation of `Map`. 
     - `ConcurrentHashMap` is backed by a hash table.
     - In Java 8, it uses CAS and `synchronized` to lock only specific bucket nodes when needed. Therefore, different threads can work on different buckets at the same time, which gives better performance than locking the entire map.
     -  Its basic operations are usually `O(1)`. 
     -  It does not allow `null` keys or `null` values.

9. String vs StringBuilder vs StringBuffer

   - `String` is immutable. Once a `String` object is created, its value cannot be changed. If we modify a `String`, Java creates a new object.
   - `StringBuilder` is mutable. It is faster when we need to modify a string many times. But it is not thread-safe.
   - `StringBuffer` is also mutable. It is thread-safe because its methods are synchronized. But it is usually slower than `StringBuilder`.

10. why we need to override the hashcode and equals method at the same time

    - `hashCode()` is used by hash-based collections like `HashMap` and `HashSet` to decide where to store an object.
    - `equals()` is used to check whether two objects are logically the same.
    - In `HashMap` and `HashSet`, Java first uses `hashCode()` to find the bucket, and then uses `equals()` to compare the objects inside that bucket.
    - If we only override `hashCode()` but do not override `equals()`, two objects with the same content may still be considered different objects.
    - If we only override `equals()` but do not override `hashCode()`, two equal objects may have different hash codes, so they may be stored in different buckets.

    

11. play around the common data structure apis (map, set, queue, list), write some practice codes

```java
import java.util.*;

public class Solution {
    public static void main(String[] args) {

        // List: allows duplicates and keeps insertion order
        List<String> names = new ArrayList<>();
        names.add("Alice");
        names.add("Bob");
        names.add("Alice");

        System.out.println("List:");
        for (String name : names) {
            System.out.println(name);
        }

        // Set: does not allow duplicates
        Set<String> uniqueNames = new HashSet<>();
        uniqueNames.add("Alice");
        uniqueNames.add("Bob");
        uniqueNames.add("Alice");

        System.out.println("Set:");
        for (String name : uniqueNames) {
            System.out.println(name);
        }

        // Map: stores key-value pairs
        Map<String, Integer> scoreMap = new HashMap<>();
        scoreMap.put("Alice", 90);
        scoreMap.put("Bob", 85);
        scoreMap.put("Alice", 95); // update Alice's score

        System.out.println("Map:");
        for (Map.Entry<String, Integer> entry : scoreMap.entrySet()) {
            System.out.println(entry.getKey() + ": " + entry.getValue());
        }

        // Queue: first in, first out
        Queue<String> queue = new LinkedList<>();
        queue.offer("Task1");
        queue.offer("Task2");
        queue.offer("Task3");

        System.out.println("Queue:");
        while (!queue.isEmpty()) {
            System.out.println(queue.poll());
        }
    }
}
```



