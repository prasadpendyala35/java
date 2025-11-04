# JDBC (Java Database Connectivity)

## 📋 Learning Objectives

- Understand JDBC architecture and drivers
- Master connection management
- Learn Statement, PreparedStatement, and CallableStatement
- Work with ResultSet and metadata
- Understand transaction management
- Learn connection pooling

## 🎯 Key Concepts

### 1. JDBC Architecture
```
Application → JDBC API → JDBC Driver Manager → Database Driver → Database
```

**JDBC Drivers Types:**
- Type 1: JDBC-ODBC Bridge
- Type 2: Native-API Driver
- Type 3: Network Protocol Driver
- Type 4: Thin Driver (Pure Java)

### 2. Establishing Connection
```java
import java.sql.*;

// Load driver (optional in JDBC 4.0+)
Class.forName("com.mysql.cj.jdbc.Driver");

// Create connection
String url = "jdbc:mysql://localhost:3306/mydb";
String username = "root";
String password = "password";

Connection conn = DriverManager.getConnection(url, username, password);

// Always close connection
conn.close();
```

### 3. Statement
```java
// Create statement
Statement stmt = conn.createStatement();

// Execute query
ResultSet rs = stmt.executeQuery("SELECT * FROM users");

while (rs.next()) {
    int id = rs.getInt("id");
    String name = rs.getString("name");
    System.out.println(id + ": " + name);
}

// Execute update
int rowsAffected = stmt.executeUpdate(
    "INSERT INTO users (name, email) VALUES ('John', 'john@example.com')"
);

// Close resources
rs.close();
stmt.close();
```

### 4. PreparedStatement (Preferred)
```java
// Prepared statement with parameters
String sql = "INSERT INTO users (name, email, age) VALUES (?, ?, ?)";
PreparedStatement pstmt = conn.prepareStatement(sql);

pstmt.setString(1, "Alice");
pstmt.setString(2, "alice@example.com");
pstmt.setInt(3, 25);

int rows = pstmt.executeUpdate();

// Query with parameters
String query = "SELECT * FROM users WHERE age > ?";
PreparedStatement pstmt = conn.prepareStatement(query);
pstmt.setInt(1, 18);

ResultSet rs = pstmt.executeQuery();
```

### 5. CallableStatement (Stored Procedures)
```java
// Call stored procedure
String sql = "{call getUserById(?)}";
CallableStatement cstmt = conn.prepareCall(sql);
cstmt.setInt(1, 101);

ResultSet rs = cstmt.executeQuery();

// Stored procedure with OUT parameter
String sql = "{call getTotalUsers(?)}";
CallableStatement cstmt = conn.prepareCall(sql);
cstmt.registerOutParameter(1, Types.INTEGER);
cstmt.execute();

int total = cstmt.getInt(1);
```

### 6. Transaction Management
```java
try {
    // Disable auto-commit
    conn.setAutoCommit(false);
    
    // Execute multiple statements
    stmt.executeUpdate("UPDATE accounts SET balance = balance - 100 WHERE id = 1");
    stmt.executeUpdate("UPDATE accounts SET balance = balance + 100 WHERE id = 2");
    
    // Commit transaction
    conn.commit();
    
} catch (SQLException e) {
    // Rollback on error
    conn.rollback();
    e.printStackTrace();
} finally {
    conn.setAutoCommit(true);
}
```

### 7. Batch Processing
```java
Statement stmt = conn.createStatement();

// Add to batch
stmt.addBatch("INSERT INTO users (name) VALUES ('User1')");
stmt.addBatch("INSERT INTO users (name) VALUES ('User2')");
stmt.addBatch("INSERT INTO users (name) VALUES ('User3')");

// Execute batch
int[] results = stmt.executeBatch();
```

### 8. Database Metadata
```java
DatabaseMetaData dbmd = conn.getMetaData();

System.out.println("Database: " + dbmd.getDatabaseProductName());
System.out.println("Version: " + dbmd.getDatabaseProductVersion());
System.out.println("Driver: " + dbmd.getDriverName());

// Get tables
ResultSet tables = dbmd.getTables(null, null, "%", null);
while (tables.next()) {
    System.out.println(tables.getString("TABLE_NAME"));
}
```

### 9. Connection Pooling (HikariCP)
```java
// Add HikariCP dependency
HikariConfig config = new HikariConfig();
config.setJdbcUrl("jdbc:mysql://localhost:3306/mydb");
config.setUsername("root");
config.setPassword("password");
config.setMaximumPoolSize(10);

HikariDataSource dataSource = new HikariDataSource(config);

// Get connection from pool
Connection conn = dataSource.getConnection();

// Use connection...

// Return to pool
conn.close();
```

### 10. Best Practices
```java
// Try-with-resources (auto-close)
String sql = "SELECT * FROM users";

try (Connection conn = DriverManager.getConnection(url, user, pass);
     PreparedStatement pstmt = conn.prepareStatement(sql);
     ResultSet rs = pstmt.executeQuery()) {
    
    while (rs.next()) {
        // Process results
    }
} catch (SQLException e) {
    e.printStackTrace();
}
```

## 💻 Practice Exercises

1. Create a CRUD application for managing students
2. Implement a banking system with transaction management
3. Build a user authentication system
4. Create a connection pool wrapper
5. Implement batch insert for large datasets
6. Build a simple ORM layer
7. Create a database backup utility

## 📚 Additional Resources

- JDBC Tutorial (Oracle)
- JDBC Best Practices
- Connection pooling strategies
- SQL injection prevention

## ⏭️ Next Topic

After mastering JDBC, move to [Networking](../02-networking/)
