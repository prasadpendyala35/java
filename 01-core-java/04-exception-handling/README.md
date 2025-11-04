# Exception Handling

## 📋 Learning Objectives

- Understand exception hierarchy in Java
- Master try-catch-finally blocks
- Learn to create custom exceptions
- Understand checked vs unchecked exceptions
- Learn exception handling best practices

## 🎯 Key Concepts

### 1. Exception Hierarchy
```
Throwable
├── Error (JVM errors, not to be caught)
└── Exception
    ├── RuntimeException (Unchecked)
    │   ├── NullPointerException
    │   ├── ArrayIndexOutOfBoundsException
    │   └── ArithmeticException
    └── IOException (Checked)
        ├── FileNotFoundException
        └── SQLException
```

### 2. Try-Catch-Finally
```java
try {
    // Code that might throw exception
    int result = 10 / 0;
} catch (ArithmeticException e) {
    System.out.println("Cannot divide by zero: " + e.getMessage());
} catch (Exception e) {
    System.out.println("General exception: " + e.getMessage());
} finally {
    // Always executes
    System.out.println("Cleanup code");
}
```

### 3. Try-with-Resources
```java
try (FileReader fr = new FileReader("file.txt");
     BufferedReader br = new BufferedReader(fr)) {
    String line = br.readLine();
} catch (IOException e) {
    e.printStackTrace();
}
// Resources automatically closed
```

### 4. Throwing Exceptions
```java
public void checkAge(int age) {
    if (age < 18) {
        throw new IllegalArgumentException("Age must be 18 or older");
    }
}

public void readFile(String path) throws IOException {
    FileReader file = new FileReader(path);
}
```

### 5. Custom Exceptions
```java
public class InsufficientFundsException extends Exception {
    private double amount;
    
    public InsufficientFundsException(double amount) {
        super("Insufficient funds: " + amount);
        this.amount = amount;
    }
    
    public double getAmount() {
        return amount;
    }
}
```

## 💻 Practice Exercises

1. Create a program with proper exception handling for file operations
2. Implement a custom exception for a banking application
3. Write a program demonstrating multiple catch blocks
4. Create a number validation program with exception handling
5. Implement try-with-resources for database connections

## 📚 Additional Resources

- Effective Java - Chapter on Exceptions
- Oracle Exception Handling Tutorial
- Best practices for exception handling

## ⏭️ Next Topic

After mastering exception handling, move to [File I/O](../05-file-io/)
