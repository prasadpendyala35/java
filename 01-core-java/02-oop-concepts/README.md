# Object-Oriented Programming (OOP) Concepts

## 📋 Learning Objectives

- Understand classes and objects
- Master the four pillars of OOP
- Learn constructors and destructors
- Work with access modifiers
- Understand method overloading and overriding

## 🎯 Key Concepts

### 1. Classes and Objects
```java
public class Car {
    // Fields (attributes)
    private String brand;
    private int year;
    
    // Constructor
    public Car(String brand, int year) {
        this.brand = brand;
        this.year = year;
    }
    
    // Method
    public void displayInfo() {
        System.out.println(brand + " - " + year);
    }
}

// Creating object
Car myCar = new Car("Toyota", 2023);
```

### 2. Encapsulation
- Private fields with public getters/setters
- Data hiding and protection

### 3. Inheritance
```java
public class Vehicle {
    protected String type;
    
    public void start() {
        System.out.println("Vehicle starting...");
    }
}

public class Car extends Vehicle {
    @Override
    public void start() {
        System.out.println("Car starting...");
    }
}
```

### 4. Polymorphism
- **Method Overloading**: Same method name, different parameters
- **Method Overriding**: Redefining parent class methods

### 5. Abstraction
```java
abstract class Animal {
    abstract void makeSound();
    
    public void sleep() {
        System.out.println("Sleeping...");
    }
}

class Dog extends Animal {
    void makeSound() {
        System.out.println("Bark!");
    }
}
```

### 6. Interfaces
```java
interface Drawable {
    void draw();
}

class Circle implements Drawable {
    public void draw() {
        System.out.println("Drawing circle");
    }
}
```

## 💻 Practice Exercises

1. Create a class hierarchy for a school management system (Student, Teacher, Staff)
2. Implement a banking system with Account, SavingsAccount, and CheckingAccount classes
3. Design an interface for shapes and implement it for different shapes
4. Create a program demonstrating all four OOP pillars
5. Build a simple library management system

## 📚 Additional Resources

- Effective Java by Joshua Bloch
- Head First Design Patterns
- Java OOP best practices

## ⏭️ Next Topic

After mastering OOP, move to [Data Structures](../03-data-structures/)
