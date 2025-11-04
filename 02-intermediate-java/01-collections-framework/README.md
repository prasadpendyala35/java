# Collections Framework

## 📋 Learning Objectives

- Understand the Collections Framework hierarchy
- Master List, Set, Map, and Queue interfaces
- Learn when to use each collection type
- Understand performance characteristics
- Master iteration and sorting techniques

## 🎯 Key Concepts

### 1. Collections Hierarchy
```
Collection
├── List (ordered, allows duplicates)
│   ├── ArrayList
│   ├── LinkedList
│   └── Vector
├── Set (unique elements)
│   ├── HashSet
│   ├── LinkedHashSet
│   └── TreeSet
└── Queue (FIFO operations)
    ├── PriorityQueue
    └── ArrayDeque

Map (key-value pairs)
├── HashMap
├── LinkedHashMap
├── TreeMap
└── Hashtable
```

### 2. List Interface
```java
// ArrayList - Dynamic array
List<String> arrayList = new ArrayList<>();
arrayList.add("Apple");
arrayList.add("Banana");
arrayList.get(0);
arrayList.remove(0);

// LinkedList - Doubly linked list
List<String> linkedList = new LinkedList<>();
linkedList.addFirst("First");
linkedList.addLast("Last");
```

### 3. Set Interface
```java
// HashSet - No order, fast operations
Set<String> hashSet = new HashSet<>();
hashSet.add("A");
hashSet.add("B");
hashSet.add("A"); // Duplicate ignored

// TreeSet - Sorted order
Set<String> treeSet = new TreeSet<>();
treeSet.add("C");
treeSet.add("A");
treeSet.add("B");
// Output: [A, B, C]
```

### 4. Map Interface
```java
// HashMap - Key-value pairs
Map<String, Integer> map = new HashMap<>();
map.put("Alice", 25);
map.put("Bob", 30);
map.get("Alice"); // 25
map.containsKey("Alice"); // true

// Iterate over map
for (Map.Entry<String, Integer> entry : map.entrySet()) {
    System.out.println(entry.getKey() + ": " + entry.getValue());
}
```

### 5. Queue Interface
```java
// PriorityQueue - Natural ordering
Queue<Integer> pq = new PriorityQueue<>();
pq.offer(3);
pq.offer(1);
pq.offer(2);
pq.poll(); // Returns 1

// ArrayDeque - Double-ended queue
Deque<String> deque = new ArrayDeque<>();
deque.offerFirst("A");
deque.offerLast("B");
```

### 6. Common Operations
```java
// Sorting
Collections.sort(list);
Collections.reverse(list);

// Searching
int index = Collections.binarySearch(list, "key");

// Utilities
Collections.max(list);
Collections.min(list);
Collections.shuffle(list);
```

## 💻 Practice Exercises

1. Implement a phonebook using HashMap
2. Remove duplicates from a list using Set
3. Create a priority queue for task scheduling
4. Implement a LRU cache using LinkedHashMap
5. Sort a list of custom objects using Comparator
6. Find the intersection of two sets
7. Implement frequency counter using Map

## 📚 Additional Resources

- Java Collections Framework Overview
- Effective Java - Collections chapter
- Performance comparison of collections

## ⏭️ Next Topic

After mastering collections, move to [Generics](../02-generics/)
