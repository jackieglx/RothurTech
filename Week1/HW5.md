1. How to create a thread? 4 ways, write code

```java
import java.util.concurrent.*;

public class CreateThreadDemo {
    public static void main(String[] args) throws Exception {

        // Way 1: extends Thread
        Thread thread1 = new MyThread();
        thread1.start();

        // Way 2: implements Runnable
        Thread thread2 = new Thread(new MyRunnable());
        thread2.start();

        // Way 3: Callable + FutureTask
        FutureTask<String> futureTask = new FutureTask<>(new MyCallable());
        Thread thread3 = new Thread(futureTask);
        thread3.start();
        System.out.println(futureTask.get());

        // Way 4: ExecutorService / Thread Pool
        ExecutorService executorService = Executors.newFixedThreadPool(2);
        executorService.submit(() -> {
            System.out.println("Thread created by thread pool");
        });

        executorService.shutdown();
    }
}

class MyThread extends Thread {
    @Override
    public void run() {
        System.out.println("Thread created by extending Thread");
    }
}

class MyRunnable implements Runnable {
    @Override
    public void run() {
        System.out.println("Thread created by implementing Runnable");
    }
}

class MyCallable implements Callable<String> {
    @Override
    public String call() {
        return "Thread created by Callable";
    }
}
```

------

2. Thread lifecycle: how does thread transfer from one state to another?

A Java thread has several states: `NEW`, `RUNNABLE`, `BLOCKED`, `WAITING`, `TIMED_WAITING`, and `TERMINATED`.
 A thread moves from `NEW` to `RUNNABLE` after `start()`, may enter `BLOCKED` when waiting for a lock, may enter `WAITING` or `TIMED_WAITING` by `wait()`, `join()`, or `sleep()`, and finally becomes `TERMINATED` when `run()` finishes.

------

3. How does thread pool work?

A thread pool manages a group of reusable worker threads to execute submitted tasks.
 When a task is submitted, the thread pool either uses an available thread, creates a new thread, puts the task into a queue, or rejects the task based on its configuration.

------

4. Potential problem of newCachedThreadPool and newFixedThreadPool

`newCachedThreadPool()` can create too many threads because its maximum thread count is very large, which may cause high memory usage or `OutOfMemoryError`.
 `newFixedThreadPool()` uses an unbounded task queue, so too many waiting tasks may accumulate and also cause memory problems.

------

5. What is Future?

`Future` represents the result of an asynchronous task.
 We can use `get()` to wait for the result, but it is blocking and not very flexible for chaining multiple async tasks.

------

6. What is CompletableFuture?

`CompletableFuture` is an enhanced version of `Future` for asynchronous programming.
 It supports non-blocking task chaining, result transformation, exception handling, and combining multiple async tasks.

------

7. Future vs CompletableFuture

`Future` can only get the result by calling blocking methods like `get()`.
 `CompletableFuture` supports callback-style programming, such as `thenApply()`, `thenAccept()`, `thenCompose()`, and `exceptionally()`.

------

8. Lock vs synchronized

`synchronized` is a built-in Java keyword that automatically locks and releases the monitor lock.
 `Lock` is more flexible because it supports manual lock control, `tryLock()`, interruptible locking, and multiple `Condition` objects.

------

9. What are wait(), notify(), notifyAll(), join()?

`wait()`, `notify()`, and `notifyAll()` are used for thread communication and must be called inside a synchronized block or method.
 `join()` is used to make one thread wait until another thread finishes execution.

------

10. Commonly used CompletableFuture APIs with demo code

runAsync / supplyAsync

```java
import java.util.concurrent.*;

public class CompletableFutureRunSupplyDemo {
    public static void main(String[] args) {
        // runAsync: no return value
        CompletableFuture<Void> future1 = CompletableFuture.runAsync(() -> {
            System.out.println("Run async task");
        });

        // supplyAsync: has return value
        CompletableFuture<String> future2 = CompletableFuture.supplyAsync(() -> {
            return "Hello CompletableFuture";
        });

        future1.join();
        System.out.println(future2.join());
    }
}
```

------

thenApply / thenApplyAsync

```java
import java.util.concurrent.*;

public class CompletableFutureThenApplyDemo {
    public static void main(String[] args) {
        CompletableFuture<Integer> future1 = CompletableFuture.supplyAsync(() -> {
            return 10;
        }).thenApply(num -> {
            return num * 2;
        });

        System.out.println(future1.join()); // 20

        CompletableFuture<Integer> future2 = CompletableFuture.supplyAsync(() -> {
            return 10;
        }).thenApplyAsync(num -> {
            return num * 3;
        });

        System.out.println(future2.join()); // 30
    }
}
```

------

handle / exceptionally

```java
import java.util.concurrent.*;

public class CompletableFutureExceptionDemo {
    public static void main(String[] args) {
        // handle: runs whether the task succeeds or fails
        CompletableFuture<Integer> future1 = CompletableFuture.supplyAsync(() -> {
            int result = 10 / 0;
            return result;
        }).handle((result, exception) -> {
            if (exception != null) {
                return -1;
            }
            return result;
        });

        System.out.println(future1.join()); // -1

        // exceptionally: runs only when there is an exception
        CompletableFuture<Integer> future2 = CompletableFuture.supplyAsync(() -> {
            int result = 10 / 0;
            return result;
        }).exceptionally(exception -> {
            return -1;
        });

        System.out.println(future2.join()); // -1
    }
}
```

------

thenCompose / thenCombine

```java
import java.util.concurrent.*;

public class CompletableFutureComposeCombineDemo {
    public static void main(String[] args) {
        // thenCompose: used when the next async task depends on the previous result
        CompletableFuture<String> future1 = CompletableFuture.supplyAsync(() -> {
            return "userId: 1001";
        }).thenCompose(userId -> {
            return CompletableFuture.supplyAsync(() -> {
                return "User info for " + userId;
            });
        });

        System.out.println(future1.join());

        // thenCombine: used to combine two independent async tasks
        CompletableFuture<Integer> priceFuture = CompletableFuture.supplyAsync(() -> {
            return 100;
        });

        CompletableFuture<Integer> discountFuture = CompletableFuture.supplyAsync(() -> {
            return 20;
        });

        CompletableFuture<Integer> finalPriceFuture = priceFuture.thenCombine(discountFuture, (price, discount) -> {
            return price - discount;
        });

        System.out.println(finalPriceFuture.join()); // 80
    }
}
```

------

allOf / anyOf

```java
import java.util.concurrent.*;

public class CompletableFutureAllAnyDemo {
    public static void main(String[] args) {
        CompletableFuture<String> future1 = CompletableFuture.supplyAsync(() -> {
            return "Task 1";
        });

        CompletableFuture<String> future2 = CompletableFuture.supplyAsync(() -> {
            return "Task 2";
        });

        CompletableFuture<String> future3 = CompletableFuture.supplyAsync(() -> {
            return "Task 3";
        });

        // allOf: waits for all tasks to finish
        CompletableFuture<Void> allFuture = CompletableFuture.allOf(future1, future2, future3);

        allFuture.join();

        System.out.println(future1.join());
        System.out.println(future2.join());
        System.out.println(future3.join());

        // anyOf: returns when any one task finishes
        CompletableFuture<Object> anyFuture = CompletableFuture.anyOf(future1, future2, future3);

        System.out.println(anyFuture.join());
    }
}
```

------

thenAccept / thenRun / whenComplete

```java
import java.util.concurrent.*;

public class CompletableFutureOtherApiDemo {
    public static void main(String[] args) {
        // thenAccept: uses the result, but returns no value
        CompletableFuture<Void> future1 = CompletableFuture.supplyAsync(() -> {
            return "Alice";
        }).thenAccept(name -> {
            System.out.println("Name: " + name);
        });

        future1.join();

        // thenRun: does not use the previous result
        CompletableFuture<Void> future2 = CompletableFuture.supplyAsync(() -> {
            return "Hello";
        }).thenRun(() -> {
            System.out.println("Previous task finished");
        });

        future2.join();

        // whenComplete: observes the result or exception, but does not change the result
        CompletableFuture<String> future3 = CompletableFuture.supplyAsync(() -> {
            return "Success";
        }).whenComplete((result, exception) -> {
            if (exception == null) {
                System.out.println("Result: " + result);
            } else {
                System.out.println("Exception: " + exception.getMessage());
            }
        });

        System.out.println(future3.join());
    }
}
```

------

CompletableFuture API summary

`runAsync()` runs an async task without a return value, while `supplyAsync()` runs an async task with a return value.
 `thenApply()`, `thenCompose()`, `thenCombine()`, `handle()`, `exceptionally()`, `allOf()`, and `anyOf()` are commonly used to transform results, chain tasks, combine tasks, and handle exceptions.