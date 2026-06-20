

# what's singleton 

Singleton Pattern ensures a class has **only one instance** in the entire application and provides a **global access point** to that instance.



# Why singleton

The Singleton Pattern is suitable when an object is expensive to create, needs to be shared globally, and logically should only have one instance.

For shared resources such as a configuration manager, thread pool, or cache client, it can reduce memory usage and initialization overhead.



单例模式可以保证一个类在系统中只有一个实例，避免重复创建对象。

对于配置中心、线程池、缓存客户端等共享资源，可以减少内存和初始化开销。



# Lazy loading 

Lazy loading means the singleton instance is not created when the class is loaded. It is created only when it is used for the first time.

单例模式中的 **Lazy Loading（懒加载）**，指的是：**对象不会在类加载时立刻创建，而是在第一次调用时才创建。**

```java
public class Singleton {

    private static volatile Singleton instance;

    private Singleton() {}

    public static Singleton getInstance() {
        if (instance == null) {
            synchronized (Singleton.class) {
                if (instance == null) {
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}
```

The first `if` check avoids entering the `synchronized` block every time after the instance has already been created, which improves performance.

The second `if` check is necessary because multiple threads may pass the first check at the same time. After a thread acquires the lock, it must check again whether another thread has already created the instance. Without the second check, multiple threads could create multiple instances one after another, breaking the Singleton Pattern.

第一个 `if` 是为了避免实例创建后每次调用都进入 `synchronized`，提升性能。

第二个 `if` 是因为多个线程可能同时通过第一次判断，拿到锁后必须再次确认实例是否已被其他线程创建。如果没有第二次判断，多个线程会依次创建多个对象，破坏单例。



```java
public class Singleton {

    private Singleton() {}

    private static class Holder {
        private static final Singleton INSTANCE = new Singleton();
    }

    public static Singleton getInstance() {
        return Holder.INSTANCE;
    }
}
```



# Eager Loading

Eager loading means the singleton instance is created when the class is loaded, instead of waiting until it is used for the first time.

Eager Loading（饿汉式）指的是：类加载时就立即创建单例对象，而不是等第一次使用时再创建。

```java
public class Singleton {

    private static final Singleton INSTANCE = new Singleton();

    private Singleton() {}

    public static Singleton getInstance() {
        return INSTANCE;
    }
}
```

