# Data Structures in Java

## 📋 Learning Objectives

- Master arrays and multi-dimensional arrays
- Understand String manipulation
- Learn StringBuilder and StringBuffer
- Work with basic data structure implementations

## 🎯 Key Concepts

### 1. Arrays
```java
// Single-dimensional array
int[] numbers = {1, 2, 3, 4, 5};
int[] scores = new int[10];

// Multi-dimensional array
int[][] matrix = new int[3][3];
int[][] table = {{1, 2}, {3, 4}, {5, 6}};

// Array operations
Arrays.sort(numbers);
int index = Arrays.binarySearch(numbers, 3);
```

### 2. Strings
```java
String str = "Hello, World!";

// String methods
int length = str.length();
String upper = str.toUpperCase();
String sub = str.substring(0, 5);
boolean contains = str.contains("World");
String[] parts = str.split(",");
```

### 3. StringBuilder and StringBuffer
```java
// Mutable strings
StringBuilder sb = new StringBuilder("Hello");
sb.append(" World");
sb.insert(5, ",");
sb.delete(0, 5);

// Thread-safe alternative
StringBuffer sbf = new StringBuffer("Hello");
```

### 4. Common Array Algorithms
- Searching (Linear, Binary)
- Sorting (Bubble, Selection, Insertion)
- Array rotation
- Finding duplicates
- Two-pointer technique

### 5. Matrix Operations
```java
// Matrix traversal
for (int i = 0; i < matrix.length; i++) {
    for (int j = 0; j < matrix[i].length; j++) {
        System.out.print(matrix[i][j] + " ");
    }
    System.out.println();
}
```

## 💻 Practice Exercises

1. Implement array rotation algorithms
2. Find the largest and smallest elements in an array
3. Reverse a string without using built-in methods
4. Check if a string is a palindrome
5. Implement matrix addition and multiplication
6. Find duplicates in an array
7. Merge two sorted arrays

## 📚 Additional Resources

- Introduction to Algorithms (CLRS)
- Data Structures and Algorithms in Java
- LeetCode array problems

## ⏭️ Next Topic

After mastering data structures, move to [Exception Handling](../04-exception-handling/)
