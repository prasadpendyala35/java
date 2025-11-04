# File I/O in Java

## 📋 Learning Objectives

- Understand File and Path classes
- Master byte and character streams
- Learn buffered I/O operations
- Work with serialization and deserialization
- Understand NIO.2 (New I/O)

## 🎯 Key Concepts

### 1. File Class
```java
import java.io.File;

File file = new File("example.txt");

// File operations
boolean exists = file.exists();
boolean created = file.createNewFile();
boolean deleted = file.delete();
long size = file.length();
```

### 2. Reading Files
```java
// Using FileReader and BufferedReader
try (BufferedReader br = new BufferedReader(new FileReader("file.txt"))) {
    String line;
    while ((line = br.readLine()) != null) {
        System.out.println(line);
    }
} catch (IOException e) {
    e.printStackTrace();
}

// Using Files.readAllLines (Java 7+)
List<String> lines = Files.readAllLines(Paths.get("file.txt"));
```

### 3. Writing Files
```java
// Using FileWriter and BufferedWriter
try (BufferedWriter bw = new BufferedWriter(new FileWriter("output.txt"))) {
    bw.write("Hello, World!");
    bw.newLine();
    bw.write("Second line");
} catch (IOException e) {
    e.printStackTrace();
}

// Using Files.write (Java 7+)
List<String> lines = Arrays.asList("Line 1", "Line 2");
Files.write(Paths.get("output.txt"), lines);
```

### 4. Binary I/O
```java
// Reading binary data
try (FileInputStream fis = new FileInputStream("data.bin");
     DataInputStream dis = new DataInputStream(fis)) {
    int intValue = dis.readInt();
    double doubleValue = dis.readDouble();
}

// Writing binary data
try (FileOutputStream fos = new FileOutputStream("data.bin");
     DataOutputStream dos = new DataOutputStream(fos)) {
    dos.writeInt(42);
    dos.writeDouble(3.14);
}
```

### 5. Serialization
```java
// Serializable class
class Person implements Serializable {
    private String name;
    private int age;
}

// Serialize object
try (ObjectOutputStream oos = new ObjectOutputStream(
        new FileOutputStream("person.ser"))) {
    Person person = new Person("John", 30);
    oos.writeObject(person);
}

// Deserialize object
try (ObjectInputStream ois = new ObjectInputStream(
        new FileInputStream("person.ser"))) {
    Person person = (Person) ois.readObject();
}
```

### 6. NIO.2 (Path and Files)
```java
import java.nio.file.*;

Path path = Paths.get("example.txt");

// File operations
boolean exists = Files.exists(path);
long size = Files.size(path);
Files.copy(source, target);
Files.move(source, target);
Files.delete(path);

// Directory operations
Files.createDirectories(Paths.get("dir/subdir"));
```

## 💻 Practice Exercises

1. Create a program to copy a file
2. Implement a text file analyzer (word count, line count)
3. Write a program to merge multiple text files
4. Create a simple contact management system with file persistence
5. Implement object serialization for saving/loading game state
6. Build a CSV file reader and writer

## 📚 Additional Resources

- Java I/O Tutorial (Oracle)
- Java NIO.2 File System API
- Best practices for file handling

## ⏭️ Next Topic

After mastering Core Java, move to [Intermediate Java - Collections Framework](../../02-intermediate-java/01-collections-framework/)
