# Generics

## 📋 Learning Objectives

- Understand type parameters and generic classes
- Learn bounded type parameters
- Master wildcards (?, extends, super)
- Understand type erasure
- Learn generic methods and interfaces

## 🎯 Key Concepts

### 1. Generic Classes
```java
// Generic class
public class Box<T> {
    private T content;
    
    public void set(T content) {
        this.content = content;
    }
    
    public T get() {
        return content;
    }
}

// Usage
Box<String> stringBox = new Box<>();
stringBox.set("Hello");
String value = stringBox.get(); // No casting needed
```

### 2. Multiple Type Parameters
```java
public class Pair<K, V> {
    private K key;
    private V value;
    
    public Pair(K key, V value) {
        this.key = key;
        this.value = value;
    }
    
    public K getKey() { return key; }
    public V getValue() { return value; }
}

// Usage
Pair<String, Integer> pair = new Pair<>("Age", 25);
```

### 3. Generic Methods
```java
public class Util {
    // Generic method
    public static <T> void printArray(T[] array) {
        for (T element : array) {
            System.out.println(element);
        }
    }
    
    // Generic method with return type
    public static <T> T getFirst(List<T> list) {
        return list.isEmpty() ? null : list.get(0);
    }
}

// Usage
String[] strings = {"A", "B", "C"};
Util.printArray(strings);
```

### 4. Bounded Type Parameters
```java
// Upper bound (extends)
public class NumberBox<T extends Number> {
    private T number;
    
    public double square() {
        return number.doubleValue() * number.doubleValue();
    }
}

// Multiple bounds
public <T extends Number & Comparable<T>> T findMax(T a, T b) {
    return a.compareTo(b) > 0 ? a : b;
}
```

### 5. Wildcards
```java
// Unbounded wildcard
public void printList(List<?> list) {
    for (Object obj : list) {
        System.out.println(obj);
    }
}

// Upper bounded wildcard (extends)
public double sumOfList(List<? extends Number> list) {
    double sum = 0.0;
    for (Number num : list) {
        sum += num.doubleValue();
    }
    return sum;
}

// Lower bounded wildcard (super)
public void addNumbers(List<? super Integer> list) {
    list.add(1);
    list.add(2);
}
```

### 6. Generic Interfaces
```java
public interface Comparable<T> {
    int compareTo(T other);
}

public class Student implements Comparable<Student> {
    private String name;
    private int grade;
    
    @Override
    public int compareTo(Student other) {
        return Integer.compare(this.grade, other.grade);
    }
}
```

## 💻 Practice Exercises

1. Create a generic Stack class
2. Implement a generic method to swap two elements in an array
3. Create a generic class for a key-value cache
4. Implement a generic method to find the maximum element in a list
5. Create a generic repository pattern
6. Build a type-safe builder pattern using generics

## 📚 Additional Resources

- Java Generics Tutorial (Oracle)
- Effective Java - Generics chapter
- Understanding type erasure
- PECS (Producer Extends, Consumer Super) principle

## ⏭️ Next Topic

After mastering generics, move to [Multithreading](../03-multithreading/)
