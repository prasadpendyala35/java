# Networking in Java

## 📋 Learning Objectives

- Understand network protocols (TCP/IP, UDP)
- Master Socket programming
- Learn URL and URLConnection
- Work with HTTP clients
- Understand server socket programming
- Learn non-blocking I/O (NIO)

## 🎯 Key Concepts

### 1. IP Addresses and Ports
```java
import java.net.*;

// InetAddress
InetAddress localhost = InetAddress.getLocalHost();
System.out.println("Local IP: " + localhost.getHostAddress());

InetAddress google = InetAddress.getByName("www.google.com");
System.out.println("Google IP: " + google.getHostAddress());

// Get all IP addresses
InetAddress[] addresses = InetAddress.getAllByName("www.google.com");
for (InetAddress addr : addresses) {
    System.out.println(addr.getHostAddress());
}
```

### 2. TCP Client (Socket)
```java
try (Socket socket = new Socket("localhost", 8080)) {
    
    // Output stream to server
    PrintWriter out = new PrintWriter(socket.getOutputStream(), true);
    
    // Input stream from server
    BufferedReader in = new BufferedReader(
        new InputStreamReader(socket.getInputStream())
    );
    
    // Send data
    out.println("Hello Server");
    
    // Receive response
    String response = in.readLine();
    System.out.println("Server: " + response);
    
} catch (IOException e) {
    e.printStackTrace();
}
```

### 3. TCP Server (ServerSocket)
```java
try (ServerSocket serverSocket = new ServerSocket(8080)) {
    System.out.println("Server started on port 8080");
    
    while (true) {
        // Accept client connection
        Socket clientSocket = serverSocket.accept();
        System.out.println("Client connected: " + clientSocket.getInetAddress());
        
        // Handle client in separate thread
        new Thread(() -> handleClient(clientSocket)).start();
    }
}

private void handleClient(Socket socket) {
    try (BufferedReader in = new BufferedReader(
            new InputStreamReader(socket.getInputStream()));
         PrintWriter out = new PrintWriter(socket.getOutputStream(), true)) {
        
        String inputLine;
        while ((inputLine = in.readLine()) != null) {
            System.out.println("Received: " + inputLine);
            out.println("Echo: " + inputLine);
        }
    } catch (IOException e) {
        e.printStackTrace();
    }
}
```

### 4. UDP Communication (DatagramSocket)
```java
// UDP Server
DatagramSocket socket = new DatagramSocket(9876);
byte[] buffer = new byte[1024];

DatagramPacket packet = new DatagramPacket(buffer, buffer.length);
socket.receive(packet); // Receive

String received = new String(packet.getData(), 0, packet.getLength());
System.out.println("Received: " + received);

// Send response
InetAddress address = packet.getAddress();
int port = packet.getPort();
String response = "Hello Client";
byte[] responseData = response.getBytes();

DatagramPacket responsePacket = new DatagramPacket(
    responseData, responseData.length, address, port
);
socket.send(responsePacket);

// UDP Client
DatagramSocket socket = new DatagramSocket();
InetAddress address = InetAddress.getByName("localhost");

String message = "Hello Server";
byte[] buffer = message.getBytes();

DatagramPacket packet = new DatagramPacket(
    buffer, buffer.length, address, 9876
);
socket.send(packet);

// Receive response
byte[] responseBuffer = new byte[1024];
DatagramPacket responsePacket = new DatagramPacket(
    responseBuffer, responseBuffer.length
);
socket.receive(responsePacket);
```

### 5. URL and URLConnection
```java
// Simple URL reading
URL url = new URL("https://api.example.com/data");
try (BufferedReader reader = new BufferedReader(
        new InputStreamReader(url.openStream()))) {
    
    String line;
    while ((line = reader.readLine()) != null) {
        System.out.println(line);
    }
}

// URLConnection for more control
URLConnection connection = url.openConnection();
connection.setRequestProperty("User-Agent", "Java Client");
connection.setConnectTimeout(5000);

try (BufferedReader reader = new BufferedReader(
        new InputStreamReader(connection.getInputStream()))) {
    // Read response
}
```

### 6. HTTP Client (Java 11+)
```java
import java.net.http.*;

// Create HTTP client
HttpClient client = HttpClient.newHttpClient();

// Build request
HttpRequest request = HttpRequest.newBuilder()
    .uri(URI.create("https://api.example.com/users"))
    .header("Content-Type", "application/json")
    .GET()
    .build();

// Send request synchronously
HttpResponse<String> response = client.send(
    request, 
    HttpResponse.BodyHandlers.ofString()
);

System.out.println("Status: " + response.statusCode());
System.out.println("Body: " + response.body());

// POST request
String json = "{\"name\":\"John\",\"age\":30}";
HttpRequest postRequest = HttpRequest.newBuilder()
    .uri(URI.create("https://api.example.com/users"))
    .header("Content-Type", "application/json")
    .POST(HttpRequest.BodyPublishers.ofString(json))
    .build();

// Asynchronous request
client.sendAsync(request, HttpResponse.BodyHandlers.ofString())
    .thenApply(HttpResponse::body)
    .thenAccept(System.out::println)
    .join();
```

### 7. Multi-threaded Server
```java
class MultiThreadedServer {
    public static void main(String[] args) throws IOException {
        ExecutorService threadPool = Executors.newFixedThreadPool(10);
        
        try (ServerSocket serverSocket = new ServerSocket(8080)) {
            while (true) {
                Socket clientSocket = serverSocket.accept();
                threadPool.submit(new ClientHandler(clientSocket));
            }
        }
    }
}

class ClientHandler implements Runnable {
    private Socket socket;
    
    public ClientHandler(Socket socket) {
        this.socket = socket;
    }
    
    @Override
    public void run() {
        // Handle client communication
    }
}
```

### 8. Non-blocking I/O (NIO)
```java
import java.nio.*;
import java.nio.channels.*;

// NIO Server
Selector selector = Selector.open();
ServerSocketChannel serverChannel = ServerSocketChannel.open();
serverChannel.bind(new InetSocketAddress(8080));
serverChannel.configureBlocking(false);
serverChannel.register(selector, SelectionKey.OP_ACCEPT);

while (true) {
    selector.select();
    Set<SelectionKey> keys = selector.selectedKeys();
    
    for (SelectionKey key : keys) {
        if (key.isAcceptable()) {
            // Accept connection
        } else if (key.isReadable()) {
            // Read data
        }
    }
    keys.clear();
}
```

## 💻 Practice Exercises

1. Build a simple chat application (client-server)
2. Create an HTTP file downloader
3. Implement a multi-threaded echo server
4. Build a REST client for consuming APIs
5. Create a UDP-based time server
6. Implement a simple web server
7. Build a port scanner utility

## 📚 Additional Resources

- Java Network Programming (book)
- Oracle Networking Tutorial
- Socket programming best practices
- Understanding TCP/IP protocols

## ⏭️ Next Topic

After mastering networking, move to [Design Patterns](../03-design-patterns/)
