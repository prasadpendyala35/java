# Multithreading and Concurrency

## 📋 Learning Objectives

- Understand threads and the Thread lifecycle
- Master synchronization and thread safety
- Learn concurrent utilities (Executor, Future, etc.)
- Understand thread pools and best practices
- Learn atomic variables and locks

## 🎯 Key Concepts

### 1. Creating Threads
```java
// Method 1: Extending Thread class
class MyThread extends Thread {
    @Override
    public void run() {
        System.out.println("Thread running: " + getName());
    }
}

MyThread thread = new MyThread();
thread.start();

// Method 2: Implementing Runnable
class MyRunnable implements Runnable {
    @Override
    public void run() {
        System.out.println("Runnable running");
    }
}

Thread thread = new Thread(new MyRunnable());
thread.start();

// Method 3: Lambda expression
Thread thread = new Thread(() -> {
    System.out.println("Lambda thread");
});
thread.start();
```

### 2. Thread Lifecycle
```
New → Runnable → Running → Terminated
              ↓           ↑
           Blocked/Waiting
```

### 3. Synchronization
```java
public class Counter {
    private int count = 0;
    
    // Synchronized method
    public synchronized void increment() {
        count++;
    }
    
    // Synchronized block
    public void decrement() {
        synchronized(this) {
            count--;
        }
    }
    
    public int getCount() {
        return count;
    }
}
```

### 4. Wait and Notify
```java
class SharedResource {
    private boolean available = false;
    
    public synchronized void produce() throws InterruptedException {
        while (available) {
            wait();
        }
        // Produce item
        available = true;
        notify();
    }
    
    public synchronized void consume() throws InterruptedException {
        while (!available) {
            wait();
        }
        // Consume item
        available = false;
        notify();
    }
}
```

### 5. Executor Framework
```java
import java.util.concurrent.*;

// Thread pool
ExecutorService executor = Executors.newFixedThreadPool(5);

// Submit tasks
executor.submit(() -> {
    System.out.println("Task executed");
});

// Shutdown
executor.shutdown();

// Single thread executor
ExecutorService singleExecutor = Executors.newSingleThreadExecutor();

// Cached thread pool
ExecutorService cachedExecutor = Executors.newCachedThreadPool();
```

### 6. Callable and Future
```java
Callable<Integer> task = () -> {
    Thread.sleep(1000);
    return 42;
};

ExecutorService executor = Executors.newSingleThreadExecutor();
Future<Integer> future = executor.submit(task);

// Get result (blocks until complete)
Integer result = future.get();

// Check if done
if (future.isDone()) {
    System.out.println("Task completed");
}
```

### 7. Concurrent Collections
```java
// Thread-safe collections
ConcurrentHashMap<String, Integer> map = new ConcurrentHashMap<>();
CopyOnWriteArrayList<String> list = new CopyOnWriteArrayList<>();
BlockingQueue<String> queue = new LinkedBlockingQueue<>();
```

### 8. Locks and Atomic Variables
```java
import java.util.concurrent.locks.*;
import java.util.concurrent.atomic.*;

// ReentrantLock
Lock lock = new ReentrantLock();
lock.lock();
try {
    // Critical section
} finally {
    lock.unlock();
}

// Atomic variables
AtomicInteger counter = new AtomicInteger(0);
counter.incrementAndGet();
counter.compareAndSet(0, 1);
```

## 💻 Practice Exercises

1. Implement a producer-consumer problem using BlockingQueue
2. Create a thread-safe singleton pattern
3. Build a thread pool executor for parallel processing
4. Implement a simple web crawler using multithreading
5. Create a concurrent counter with proper synchronization
6. Solve the dining philosophers problem
7. Implement a parallel merge sort

## 📚 Additional Resources

- Java Concurrency in Practice (book)
- Oracle Concurrency Tutorial
- Understanding happens-before relationship
- Thread safety best practices

## ⏭️ Next Topic

After mastering multithreading, move to [Lambda Expressions](../04-lambda-expressions/)
