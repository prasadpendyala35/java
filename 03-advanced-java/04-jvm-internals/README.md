# JVM Internals and Performance Tuning

## 📋 Learning Objectives

- Understand JVM architecture
- Master garbage collection concepts
- Learn memory management
- Understand class loading mechanism
- Learn JVM tuning and optimization
- Master profiling and monitoring tools

## 🎯 Key Concepts

### 1. JVM Architecture
```
┌─────────────────────────────────────┐
│         Class Loader Subsystem      │
├─────────────────────────────────────┤
│         Runtime Data Areas          │
│  ┌───────────────────────────────┐  │
│  │    Method Area / Metaspace    │  │
│  ├───────────────────────────────┤  │
│  │         Heap Memory           │  │
│  │  ┌─────────┐   ┌──────────┐  │  │
│  │  │  Young  │   │   Old    │  │  │
│  │  │   Gen   │   │   Gen    │  │  │
│  │  └─────────┘   └──────────┘  │  │
│  ├───────────────────────────────┤  │
│  │      Stack (per thread)       │  │
│  ├───────────────────────────────┤  │
│  │    PC Register (per thread)   │  │
│  ├───────────────────────────────┤  │
│  │  Native Method Stack          │  │
│  └───────────────────────────────┘  │
├─────────────────────────────────────┤
│        Execution Engine             │
│  - Interpreter                      │
│  - JIT Compiler                     │
│  - Garbage Collector                │
└─────────────────────────────────────┘
```

### 2. Memory Areas

**Heap Memory:**
- Young Generation (Eden + Survivor spaces)
- Old Generation (Tenured)
- Objects allocated here

**Method Area (Metaspace in Java 8+):**
- Class metadata
- Static variables
- Constant pool

**Stack:**
- Method frames
- Local variables
- Partial results

### 3. Garbage Collection

**GC Algorithms:**

**Serial GC:**
```bash
-XX:+UseSerialGC
```
- Single-threaded
- Good for small applications

**Parallel GC (Throughput):**
```bash
-XX:+UseParallelGC
-XX:ParallelGCThreads=4
```
- Multiple GC threads
- High throughput

**CMS (Concurrent Mark Sweep):**
```bash
-XX:+UseConcMarkSweepGC
```
- Low pause times
- Deprecated in Java 9+

**G1 GC (Default in Java 9+):**
```bash
-XX:+UseG1GC
-XX:MaxGCPauseMillis=200
```
- Balanced throughput and latency
- Predictable pause times

**ZGC (Java 11+):**
```bash
-XX:+UseZGC
```
- Ultra-low latency
- Handles large heaps

### 4. GC Tuning Parameters
```bash
# Heap size
-Xms2g          # Initial heap size
-Xmx4g          # Maximum heap size

# Young generation
-Xmn1g          # Young generation size
-XX:NewRatio=2  # Ratio of old/young

# Metaspace
-XX:MetaspaceSize=256m
-XX:MaxMetaspaceSize=512m

# GC logging (Java 8)
-XX:+PrintGCDetails
-XX:+PrintGCDateStamps
-Xloggc:gc.log

# GC logging (Java 9+)
-Xlog:gc*:file=gc.log:time,uptime,level,tags
```

### 5. Memory Leak Detection
```java
// Example: Memory leak with static collection
public class MemoryLeakExample {
    private static final List<Object> list = new ArrayList<>();
    
    public void addToCache(Object obj) {
        list.add(obj); // Objects never removed
    }
}

// Fix: Use WeakHashMap or clear cache periodically
private static final Map<Key, Value> cache = 
    new WeakHashMap<>();
```

### 6. Class Loading
```java
// Class loader hierarchy
Bootstrap ClassLoader (loads JDK classes)
    ↓
Extension ClassLoader (loads ext directory)
    ↓
Application ClassLoader (loads classpath)
    ↓
Custom ClassLoader

// Custom class loader
public class CustomClassLoader extends ClassLoader {
    @Override
    public Class<?> findClass(String name) throws ClassNotFoundException {
        byte[] classData = loadClassData(name);
        return defineClass(name, classData, 0, classData.length);
    }
    
    private byte[] loadClassData(String name) {
        // Load class bytes from custom source
        return new byte[0];
    }
}
```

### 7. JIT Compilation
```bash
# JIT compiler options
-XX:+TieredCompilation
-XX:CompileThreshold=10000

# Disable JIT
-Xint

# JIT only (no interpreter)
-Xcomp
```

### 8. Monitoring Tools

**jps - List Java processes:**
```bash
jps -l
```

**jstat - JVM statistics:**
```bash
jstat -gc <pid> 1000    # GC stats every second
jstat -gcutil <pid>     # GC utilization
```

**jmap - Memory map:**
```bash
jmap -heap <pid>        # Heap summary
jmap -dump:format=b,file=heap.bin <pid>  # Heap dump
```

**jstack - Thread dump:**
```bash
jstack <pid>            # Thread dump
```

**jconsole - GUI monitoring:**
```bash
jconsole
```

**VisualVM:**
```bash
jvisualvm
```

### 9. Performance Optimization

**Object Creation:**
```java
// Avoid unnecessary object creation
String s = "Hello";     // Good
String s = new String("Hello");  // Bad

// Use StringBuilder for concatenation
StringBuilder sb = new StringBuilder();
for (int i = 0; i < 1000; i++) {
    sb.append(i);
}
```

**Collection Sizing:**
```java
// Specify initial capacity
List<String> list = new ArrayList<>(1000);
Map<String, String> map = new HashMap<>(100);
```

**Lazy Initialization:**
```java
private volatile ExpensiveObject obj;

public ExpensiveObject getObject() {
    if (obj == null) {
        synchronized(this) {
            if (obj == null) {
                obj = new ExpensiveObject();
            }
        }
    }
    return obj;
}
```

### 10. Common Performance Issues

**String Concatenation in Loops:**
```java
// Bad
String result = "";
for (int i = 0; i < 1000; i++) {
    result += i;  // Creates new String object each iteration
}

// Good
StringBuilder sb = new StringBuilder();
for (int i = 0; i < 1000; i++) {
    sb.append(i);
}
```

**Unnecessary Autoboxing:**
```java
// Bad
Long sum = 0L;
for (long i = 0; i < Integer.MAX_VALUE; i++) {
    sum += i;  // Autoboxing/unboxing overhead
}

// Good
long sum = 0L;
for (long i = 0; i < Integer.MAX_VALUE; i++) {
    sum += i;
}
```

## 💻 Practice Exercises

1. Analyze heap dumps using jmap and MAT
2. Tune GC for a high-throughput application
3. Profile an application using VisualVM
4. Identify and fix memory leaks
5. Optimize string operations in a text processor
6. Benchmark different GC algorithms
7. Create custom class loader for plugin system

## 📚 Additional Resources

- Java Performance: The Definitive Guide
- JVM specification
- GC tuning guides
- Understanding JVM memory model
- Java profiling best practices

## ⏭️ Next Topic

After mastering JVM internals, move to [Frameworks](../05-frameworks/)
