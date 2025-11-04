# Streams API

## 📋 Learning Objectives

- Understand streams and stream pipeline
- Master intermediate and terminal operations
- Learn collectors and grouping
- Understand parallel streams
- Learn reduction operations

## 🎯 Key Concepts

### 1. Creating Streams
```java
// From collections
List<String> list = Arrays.asList("a", "b", "c");
Stream<String> stream = list.stream();

// From arrays
String[] array = {"a", "b", "c"};
Stream<String> stream = Arrays.stream(array);

// Using Stream.of()
Stream<String> stream = Stream.of("a", "b", "c");

// Infinite streams
Stream<Integer> infiniteStream = Stream.iterate(0, n -> n + 1);
Stream<Double> randomStream = Stream.generate(Math::random);
```

### 2. Intermediate Operations (Lazy)
```java
List<String> names = Arrays.asList("Alice", "Bob", "Charlie", "David");

// filter - Select elements
List<String> filtered = names.stream()
    .filter(name -> name.startsWith("A"))
    .collect(Collectors.toList());

// map - Transform elements
List<Integer> lengths = names.stream()
    .map(String::length)
    .collect(Collectors.toList());

// flatMap - Flatten nested structures
List<List<String>> nested = Arrays.asList(
    Arrays.asList("a", "b"),
    Arrays.asList("c", "d")
);
List<String> flattened = nested.stream()
    .flatMap(Collection::stream)
    .collect(Collectors.toList());

// distinct - Remove duplicates
List<Integer> unique = Arrays.asList(1, 2, 2, 3, 3, 3).stream()
    .distinct()
    .collect(Collectors.toList());

// sorted - Sort elements
List<String> sorted = names.stream()
    .sorted()
    .collect(Collectors.toList());

// limit and skip
List<String> limited = names.stream()
    .limit(2)
    .collect(Collectors.toList());
```

### 3. Terminal Operations (Eager)
```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);

// collect
List<Integer> list = numbers.stream()
    .collect(Collectors.toList());

// forEach
numbers.stream()
    .forEach(System.out::println);

// count
long count = numbers.stream()
    .filter(n -> n > 2)
    .count();

// anyMatch, allMatch, noneMatch
boolean hasEven = numbers.stream()
    .anyMatch(n -> n % 2 == 0);

// findFirst, findAny
Optional<Integer> first = numbers.stream()
    .filter(n -> n > 3)
    .findFirst();

// min, max
Optional<Integer> max = numbers.stream()
    .max(Integer::compareTo);
```

### 4. Collectors
```java
List<String> names = Arrays.asList("Alice", "Bob", "Charlie");

// toList, toSet
List<String> list = names.stream()
    .collect(Collectors.toList());

// joining
String joined = names.stream()
    .collect(Collectors.joining(", "));

// groupingBy
Map<Integer, List<String>> byLength = names.stream()
    .collect(Collectors.groupingBy(String::length));

// partitioningBy
Map<Boolean, List<String>> partitioned = names.stream()
    .collect(Collectors.partitioningBy(s -> s.length() > 4));

// counting
Map<Integer, Long> counts = names.stream()
    .collect(Collectors.groupingBy(
        String::length,
        Collectors.counting()
    ));

// summarizing
IntSummaryStatistics stats = names.stream()
    .collect(Collectors.summarizingInt(String::length));
```

### 5. Reduction Operations
```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);

// reduce - Sum
int sum = numbers.stream()
    .reduce(0, (a, b) -> a + b);

// reduce - Product
int product = numbers.stream()
    .reduce(1, (a, b) -> a * b);

// reduce - Max
Optional<Integer> max = numbers.stream()
    .reduce(Integer::max);
```

### 6. Parallel Streams
```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);

// Parallel stream
long count = numbers.parallelStream()
    .filter(n -> n % 2 == 0)
    .count();

// Convert to parallel
long count2 = numbers.stream()
    .parallel()
    .filter(n -> n % 2 == 0)
    .count();
```

### 7. Common Patterns
```java
// Map-Filter-Reduce
int sumOfSquares = numbers.stream()
    .map(n -> n * n)
    .filter(n -> n > 10)
    .reduce(0, Integer::sum);

// Complex transformation
List<String> result = employees.stream()
    .filter(e -> e.getSalary() > 50000)
    .sorted(Comparator.comparing(Employee::getName))
    .map(Employee::getName)
    .collect(Collectors.toList());
```

## 💻 Practice Exercises

1. Filter and transform a list of employees
2. Group students by grade using Collectors.groupingBy
3. Find the sum, average, min, and max of a list of numbers
4. Implement word frequency counter using streams
5. Flatten a list of lists using flatMap
6. Create a custom collector
7. Compare performance of sequential vs parallel streams

## 📚 Additional Resources

- Java 8 Streams Tutorial (Oracle)
- Modern Java in Action (book)
- Stream API best practices
- Performance considerations for parallel streams

## ⏭️ Next Topic

After mastering Intermediate Java, move to [Advanced Java - JDBC](../../03-advanced-java/01-jdbc/)
