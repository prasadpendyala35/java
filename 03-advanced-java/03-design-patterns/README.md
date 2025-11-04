# Design Patterns

## 📋 Learning Objectives

- Understand Gang of Four (GoF) design patterns
- Master creational patterns
- Learn structural patterns
- Understand behavioral patterns
- Apply patterns to real-world problems
- Learn when and when not to use patterns

## 🎯 Key Concepts

## Creational Patterns
*Patterns for object creation mechanisms*

### 1. Singleton Pattern
```java
public class Singleton {
    private static volatile Singleton instance;
    
    private Singleton() {
        // Private constructor
    }
    
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

// Enum Singleton (Preferred)
public enum EnumSingleton {
    INSTANCE;
    
    public void doSomething() {
        // Business logic
    }
}
```

### 2. Factory Pattern
```java
interface Shape {
    void draw();
}

class Circle implements Shape {
    public void draw() {
        System.out.println("Drawing Circle");
    }
}

class Rectangle implements Shape {
    public void draw() {
        System.out.println("Drawing Rectangle");
    }
}

class ShapeFactory {
    public Shape getShape(String shapeType) {
        if (shapeType == null) return null;
        
        switch (shapeType.toLowerCase()) {
            case "circle": return new Circle();
            case "rectangle": return new Rectangle();
            default: return null;
        }
    }
}
```

### 3. Builder Pattern
```java
public class User {
    private final String firstName;
    private final String lastName;
    private final int age;
    private final String email;
    
    private User(Builder builder) {
        this.firstName = builder.firstName;
        this.lastName = builder.lastName;
        this.age = builder.age;
        this.email = builder.email;
    }
    
    public static class Builder {
        private String firstName;
        private String lastName;
        private int age;
        private String email;
        
        public Builder firstName(String firstName) {
            this.firstName = firstName;
            return this;
        }
        
        public Builder lastName(String lastName) {
            this.lastName = lastName;
            return this;
        }
        
        public Builder age(int age) {
            this.age = age;
            return this;
        }
        
        public Builder email(String email) {
            this.email = email;
            return this;
        }
        
        public User build() {
            return new User(this);
        }
    }
}

// Usage
User user = new User.Builder()
    .firstName("John")
    .lastName("Doe")
    .age(30)
    .email("john@example.com")
    .build();
```

### 4. Prototype Pattern
```java
public class Document implements Cloneable {
    private String content;
    
    @Override
    public Document clone() {
        try {
            return (Document) super.clone();
        } catch (CloneNotSupportedException e) {
            throw new AssertionError();
        }
    }
}
```

## Structural Patterns
*Patterns for composing classes and objects*

### 5. Adapter Pattern
```java
// Target interface
interface MediaPlayer {
    void play(String audioType, String fileName);
}

// Adaptee
class AdvancedMediaPlayer {
    void playVlc(String fileName) {
        System.out.println("Playing vlc: " + fileName);
    }
}

// Adapter
class MediaAdapter implements MediaPlayer {
    AdvancedMediaPlayer advancedPlayer;
    
    public MediaAdapter() {
        advancedPlayer = new AdvancedMediaPlayer();
    }
    
    public void play(String audioType, String fileName) {
        if (audioType.equals("vlc")) {
            advancedPlayer.playVlc(fileName);
        }
    }
}
```

### 6. Decorator Pattern
```java
interface Coffee {
    double getCost();
    String getDescription();
}

class SimpleCoffee implements Coffee {
    public double getCost() {
        return 1.0;
    }
    
    public String getDescription() {
        return "Simple coffee";
    }
}

abstract class CoffeeDecorator implements Coffee {
    protected Coffee coffee;
    
    public CoffeeDecorator(Coffee coffee) {
        this.coffee = coffee;
    }
}

class MilkDecorator extends CoffeeDecorator {
    public MilkDecorator(Coffee coffee) {
        super(coffee);
    }
    
    public double getCost() {
        return coffee.getCost() + 0.5;
    }
    
    public String getDescription() {
        return coffee.getDescription() + ", milk";
    }
}

// Usage
Coffee coffee = new SimpleCoffee();
coffee = new MilkDecorator(coffee);
```

### 7. Facade Pattern
```java
class ComputerFacade {
    private CPU cpu;
    private Memory memory;
    private HardDrive hardDrive;
    
    public ComputerFacade() {
        this.cpu = new CPU();
        this.memory = new Memory();
        this.hardDrive = new HardDrive();
    }
    
    public void start() {
        cpu.freeze();
        memory.load(BOOT_ADDRESS, hardDrive.read(BOOT_SECTOR, SECTOR_SIZE));
        cpu.jump(BOOT_ADDRESS);
        cpu.execute();
    }
}
```

## Behavioral Patterns
*Patterns for communication between objects*

### 8. Observer Pattern
```java
interface Observer {
    void update(String message);
}

class Subject {
    private List<Observer> observers = new ArrayList<>();
    
    public void attach(Observer observer) {
        observers.add(observer);
    }
    
    public void notifyObservers(String message) {
        for (Observer observer : observers) {
            observer.update(message);
        }
    }
}

class ConcreteObserver implements Observer {
    private String name;
    
    public void update(String message) {
        System.out.println(name + " received: " + message);
    }
}
```

### 9. Strategy Pattern
```java
interface PaymentStrategy {
    void pay(int amount);
}

class CreditCardStrategy implements PaymentStrategy {
    public void pay(int amount) {
        System.out.println("Paid " + amount + " using Credit Card");
    }
}

class PayPalStrategy implements PaymentStrategy {
    public void pay(int amount) {
        System.out.println("Paid " + amount + " using PayPal");
    }
}

class ShoppingCart {
    private PaymentStrategy paymentStrategy;
    
    public void setPaymentStrategy(PaymentStrategy strategy) {
        this.paymentStrategy = strategy;
    }
    
    public void checkout(int amount) {
        paymentStrategy.pay(amount);
    }
}
```

### 10. Template Method Pattern
```java
abstract class DataProcessor {
    // Template method
    public final void process() {
        readData();
        processData();
        writeData();
    }
    
    abstract void readData();
    abstract void processData();
    abstract void writeData();
}

class CSVDataProcessor extends DataProcessor {
    void readData() {
        System.out.println("Reading CSV");
    }
    
    void processData() {
        System.out.println("Processing CSV");
    }
    
    void writeData() {
        System.out.println("Writing CSV");
    }
}
```

## 💻 Practice Exercises

1. Implement a logging system using Singleton
2. Create a pizza ordering system using Factory and Builder
3. Build a text editor with undo/redo using Command pattern
4. Implement a notification system using Observer
5. Create a caching proxy using Proxy pattern
6. Build a document converter using Strategy pattern
7. Implement a state machine using State pattern

## 📚 Additional Resources

- Design Patterns: Elements of Reusable Object-Oriented Software (GoF)
- Head First Design Patterns
- Refactoring to Patterns
- sourcemaking.com/design_patterns

## ⏭️ Next Topic

After mastering design patterns, move to [JVM Internals](../04-jvm-internals/)
