# Java Frameworks

## 📋 Learning Objectives

- Understand popular Java frameworks
- Learn Spring Framework basics
- Master dependency injection
- Understand ORM with Hibernate
- Learn web development frameworks
- Explore build tools and testing frameworks

## 🎯 Key Concepts

## Spring Framework

### 1. Spring Core (IoC and DI)
```java
// Bean configuration
@Configuration
public class AppConfig {
    @Bean
    public UserService userService() {
        return new UserServiceImpl();
    }
    
    @Bean
    public UserRepository userRepository() {
        return new UserRepositoryImpl();
    }
}

// Dependency Injection
@Component
public class UserService {
    private final UserRepository repository;
    
    @Autowired
    public UserService(UserRepository repository) {
        this.repository = repository;
    }
}

// Application context
ApplicationContext context = 
    new AnnotationConfigApplicationContext(AppConfig.class);
UserService service = context.getBean(UserService.class);
```

### 2. Spring Boot
```java
@SpringBootApplication
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}

@RestController
@RequestMapping("/api/users")
public class UserController {
    @Autowired
    private UserService userService;
    
    @GetMapping("/{id}")
    public User getUser(@PathVariable Long id) {
        return userService.findById(id);
    }
    
    @PostMapping
    public User createUser(@RequestBody User user) {
        return userService.save(user);
    }
}
```

### 3. Spring Data JPA
```java
// Entity
@Entity
@Table(name = "users")
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @Column(nullable = false)
    private String name;
    
    private String email;
    
    // Getters and setters
}

// Repository
public interface UserRepository extends JpaRepository<User, Long> {
    List<User> findByName(String name);
    
    @Query("SELECT u FROM User u WHERE u.email = ?1")
    User findByEmail(String email);
}

// Service
@Service
public class UserService {
    @Autowired
    private UserRepository repository;
    
    public User save(User user) {
        return repository.save(user);
    }
    
    public Optional<User> findById(Long id) {
        return repository.findById(id);
    }
}
```

## Hibernate (ORM)

### 4. Hibernate Basics
```java
// Configuration
Configuration configuration = new Configuration();
configuration.configure("hibernate.cfg.xml");

SessionFactory sessionFactory = configuration.buildSessionFactory();
Session session = sessionFactory.openSession();

// CRUD operations
Transaction transaction = session.beginTransaction();

// Create
User user = new User("John", "john@example.com");
session.save(user);

// Read
User retrieved = session.get(User.class, 1L);

// Update
retrieved.setEmail("newemail@example.com");
session.update(retrieved);

// Delete
session.delete(retrieved);

transaction.commit();
session.close();

// HQL (Hibernate Query Language)
Query<User> query = session.createQuery(
    "FROM User WHERE name = :name", User.class
);
query.setParameter("name", "John");
List<User> users = query.list();
```

### 5. JPA Annotations
```java
@Entity
@Table(name = "orders")
public class Order {
    @Id
    @GeneratedValue
    private Long id;
    
    @ManyToOne
    @JoinColumn(name = "user_id")
    private User user;
    
    @OneToMany(mappedBy = "order", cascade = CascadeType.ALL)
    private List<OrderItem> items;
    
    @Temporal(TemporalType.TIMESTAMP)
    private Date orderDate;
    
    @Enumerated(EnumType.STRING)
    private OrderStatus status;
}
```

## Build Tools

### 6. Maven
```xml
<!-- pom.xml -->
<project>
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.example</groupId>
    <artifactId>myapp</artifactId>
    <version>1.0-SNAPSHOT</version>
    
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
            <version>2.7.0</version>
        </dependency>
        
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.13.2</version>
            <scope>test</scope>
        </dependency>
    </dependencies>
</project>
```

### 7. Gradle
```groovy
// build.gradle
plugins {
    id 'java'
    id 'org.springframework.boot' version '2.7.0'
}

group = 'com.example'
version = '1.0-SNAPSHOT'

repositories {
    mavenCentral()
}

dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-web'
    testImplementation 'junit:junit:4.13.2'
}
```

## Testing Frameworks

### 8. JUnit 5
```java
import org.junit.jupiter.api.*;
import static org.junit.jupiter.api.Assertions.*;

public class CalculatorTest {
    private Calculator calculator;
    
    @BeforeEach
    void setUp() {
        calculator = new Calculator();
    }
    
    @Test
    void testAddition() {
        assertEquals(5, calculator.add(2, 3));
    }
    
    @Test
    void testDivisionByZero() {
        assertThrows(ArithmeticException.class, 
            () -> calculator.divide(10, 0));
    }
    
    @ParameterizedTest
    @ValueSource(ints = {2, 4, 6, 8})
    void testEvenNumbers(int number) {
        assertTrue(calculator.isEven(number));
    }
}
```

### 9. Mockito
```java
import static org.mockito.Mockito.*;

public class UserServiceTest {
    @Mock
    private UserRepository repository;
    
    @InjectMocks
    private UserService service;
    
    @BeforeEach
    void setUp() {
        MockitoAnnotations.openMocks(this);
    }
    
    @Test
    void testFindUser() {
        User user = new User("John");
        when(repository.findById(1L)).thenReturn(Optional.of(user));
        
        User found = service.findById(1L);
        
        assertEquals("John", found.getName());
        verify(repository, times(1)).findById(1L);
    }
}
```

## Other Popular Frameworks

### 10. Apache Commons
```java
// StringUtils
import org.apache.commons.lang3.StringUtils;

boolean isEmpty = StringUtils.isEmpty(str);
String capitalized = StringUtils.capitalize(str);

// Collections
import org.apache.commons.collections4.CollectionUtils;

boolean empty = CollectionUtils.isEmpty(list);
Collection<String> intersection = CollectionUtils.intersection(list1, list2);
```

### 11. Logging (SLF4J + Logback)
```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class MyClass {
    private static final Logger logger = LoggerFactory.getLogger(MyClass.class);
    
    public void doSomething() {
        logger.info("Starting operation");
        logger.debug("Debug information: {}", value);
        logger.error("Error occurred", exception);
    }
}
```

### 12. Jackson (JSON Processing)
```java
import com.fasterxml.jackson.databind.ObjectMapper;

ObjectMapper mapper = new ObjectMapper();

// Object to JSON
User user = new User("John", "john@example.com");
String json = mapper.writeValueAsString(user);

// JSON to Object
User deserialized = mapper.readValue(json, User.class);

// JSON to List
List<User> users = mapper.readValue(
    jsonArray, 
    new TypeReference<List<User>>(){}
);
```

## 💻 Practice Exercises

1. Build a REST API using Spring Boot
2. Create a CRUD application with Spring Data JPA
3. Implement user authentication with Spring Security
4. Build a microservice with Spring Cloud
5. Create a web application using Spring MVC
6. Implement caching with Spring Cache and Redis
7. Build a batch processing application with Spring Batch
8. Create comprehensive unit tests with JUnit and Mockito

## 📚 Additional Resources

- Spring Framework Documentation
- Spring Boot Reference Guide
- Hibernate Documentation
- JUnit 5 User Guide
- Maven/Gradle official guides
- Baeldung Spring tutorials
- Spring in Action (book)
- Pro Spring 5 (book)

## 🎓 Congratulations!

You've completed the Java learning path from core concepts to advanced frameworks. Continue practicing and building real-world projects to master these concepts!

### Next Steps:
- Build portfolio projects
- Contribute to open-source
- Explore microservices architecture
- Learn cloud platforms (AWS, Azure, GCP)
- Study system design
- Prepare for certifications (Oracle Java SE, Spring Professional)
