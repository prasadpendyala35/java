# Lambda Expressions and Functional Programming

## 📋 Learning Objectives

- Understand lambda syntax and functional interfaces
- Master method references
- Learn built-in functional interfaces
- Understand closure and variable capture
- Write functional-style code

## 🎯 Key Concepts

### 1. Lambda Syntax
```java
// Traditional approach
Runnable runnable = new Runnable() {
    @Override
    public void run() {
        System.out.println("Running");
    }
};

// Lambda expression
Runnable lambdaRunnable = () -> System.out.println("Running");

// Lambda with parameters
Comparator<String> comparator = (s1, s2) -> s1.compareTo(s2);

// Lambda with multiple statements
Comparator<String> lengthComparator = (s1, s2) -> {
    int len1 = s1.length();
    int len2 = s2.length();
    return Integer.compare(len1, len2);
};
```

### 2. Functional Interfaces
```java
@FunctionalInterface
interface Calculator {
    int calculate(int a, int b);
}

// Using lambda
Calculator add = (a, b) -> a + b;
Calculator multiply = (a, b) -> a * b;

int sum = add.calculate(5, 3);      // 8
int product = multiply.calculate(5, 3); // 15
```

### 3. Built-in Functional Interfaces
```java
// Predicate<T> - Boolean test
Predicate<Integer> isEven = n -> n % 2 == 0;
boolean result = isEven.test(4); // true

// Function<T, R> - Transform T to R
Function<String, Integer> length = s -> s.length();
int len = length.apply("Hello"); // 5

// Consumer<T> - Accept T, return nothing
Consumer<String> printer = s -> System.out.println(s);
printer.accept("Hello");

// Supplier<T> - Return T, accept nothing
Supplier<Double> randomSupplier = () -> Math.random();
double value = randomSupplier.get();

// BiFunction<T, U, R> - Two inputs, one output
BiFunction<Integer, Integer, Integer> add = (a, b) -> a + b;
int sum = add.apply(5, 3);
```

### 4. Method References
```java
// Static method reference
Function<String, Integer> parser = Integer::parseInt;
int num = parser.apply("123");

// Instance method reference
String str = "Hello";
Supplier<String> upperCase = str::toUpperCase;

// Constructor reference
Supplier<List<String>> listSupplier = ArrayList::new;
List<String> list = listSupplier.get();

// Reference to instance method of arbitrary object
Function<String, String> toUpper = String::toUpperCase;
String result = toUpper.apply("hello");
```

### 5. Lambda with Collections
```java
List<String> names = Arrays.asList("Alice", "Bob", "Charlie");

// forEach
names.forEach(name -> System.out.println(name));
names.forEach(System.out::println);

// removeIf
names.removeIf(name -> name.startsWith("A"));

// replaceAll
names.replaceAll(String::toUpperCase);

// sort
names.sort((a, b) -> a.compareTo(b));
names.sort(String::compareTo);
```

### 6. Composing Functions
```java
Function<Integer, Integer> multiplyBy2 = x -> x * 2;
Function<Integer, Integer> add3 = x -> x + 3;

// Compose: apply add3 first, then multiplyBy2
Function<Integer, Integer> composed = multiplyBy2.compose(add3);
int result = composed.apply(5); // (5 + 3) * 2 = 16

// AndThen: apply multiplyBy2 first, then add3
Function<Integer, Integer> andThen = multiplyBy2.andThen(add3);
int result2 = andThen.apply(5); // (5 * 2) + 3 = 13
```

### 7. Variable Capture
```java
int factor = 2;
Function<Integer, Integer> multiplier = x -> x * factor;
// factor must be final or effectively final
```

## 💻 Practice Exercises

1. Create a custom functional interface for mathematical operations
2. Use lambda expressions to filter and transform a list
3. Implement a calculator using functional interfaces
4. Create a custom comparator using lambdas
5. Build a validation framework using Predicate
6. Implement function composition for data transformation pipeline
7. Create a functional approach to event handling

## 📚 Additional Resources

- Java 8 in Action (book)
- Functional Programming in Java
- Lambda expressions best practices
- Understanding closures in Java

## ⏭️ Next Topic

After mastering lambda expressions, move to [Streams API](../05-streams-api/)
