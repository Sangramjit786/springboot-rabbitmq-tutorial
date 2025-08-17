# Spring Boot 3 + RabbitMQ Tutorial  

This project demonstrates **message-driven microservices communication** using **Spring Boot 3** and **RabbitMQ**.  
It covers producer-consumer patterns, REST APIs to publish messages, and handling both **plain text** and **JSON-based structured messages**.  

---

## 📌 Features Covered  

### 1. Create and Setup Spring Boot 3 Project in IntelliJ – Connect to RabbitMQ and Configure RabbitMQ Producer  
**Objective:** Initialize a Spring Boot 3 application with RabbitMQ integration to support asynchronous messaging.  

**Implementation Steps:**  
- **Spring Boot Initialization:**  
  - Used Spring Initializr with dependencies:  
    - `spring-boot-starter-web`  
    - `spring-boot-starter-amqp`  
    - `spring-boot-devtools`  
- **Project Setup in IntelliJ IDEA:** Imported as a **Maven project** and configured Java 17+.  
- **RabbitMQ Configuration in `application.properties`:**  
  ```properties
  spring.rabbitmq.host=localhost
  spring.rabbitmq.port=5672
  spring.rabbitmq.username=guest
  spring.rabbitmq.password=guest

- Define Exchange, Queue, and Binding: Added DirectExchange, Queue, and Binding as @Bean.

- Create RabbitMQ Producer:
```java
rabbitTemplate.convertAndSend(exchange, routingKey, message);
```

**💡 Interview Insight:**
Demonstrates foundational knowledge of Spring AMQP, RabbitMQ setup, and producer configuration.

---

### 2. Create RabbitMQ Consumer

**Objective:** Implement a listener to receive messages from a RabbitMQ queue asynchronously.

**Implementation Steps:**

- Created a consumer class with @RabbitListener(queues = "queue_name").
- Implemented business logic to process received messages.
- Added logging and basic validation for flow verification.

**💡 Professional Tip:** Shows how consumers enable asynchronous decoupling and scalable architectures.

---

### 3. Create REST API to Send Message

**Objective:** Expose an endpoint to allow clients to publish messages via producer.

**Implementation Steps:**
- Defined a REST controller with @RestController.
- Created a POST endpoint /api/publish.
- Injected RabbitMQProducer and delegated message publishing.

**Code Example:**
```java
@PostMapping("/publish")
public ResponseEntity<String> sendMessage(@RequestBody String message) {
    producer.send(message);
    return ResponseEntity.ok("Message sent");
}
```

**💡 Interview Insight:** Demonstrates sync-to-async handoff, REST abstraction over messaging, and clean layering.

---

### 4. Create RabbitMQ Producer to Produce JSON Message

**Objective:** Extend producer to support structured JSON messages.

**Implementation Steps:**

- Defined a POJO (e.g., Order).
- Used Jackson (auto-configured) for JSON serialization.
- Sent JSON via RabbitTemplate:
```java
rabbitTemplate.convertAndSend(exchange, routingKey, objectMapper.writeValueAsString(order));
```

**💡 Best Practice:** Structured communication enables type safety and enterprise-grade workflows.

---

### 5. Create REST API to Send JSON Object

**Objective:** Allow external clients to send domain-specific JSON payloads.

**Implementation Steps:**

- Created a POST endpoint /api/publishJson.
- Accepted JSON request body mapped to a POJO.
- Delegated publishing to producer.

**Code Example:**
```java
@PostMapping("/publishJson")
public ResponseEntity<String> sendJson(@RequestBody Order order) {
    producer.sendOrder(order);
    return ResponseEntity.ok("Order sent");
}
```
- Added validation annotations (@NotNull, @Size) for input correctness.

**💡 Professional Value:** Demonstrates REST + JSON + Messaging integration for real-world systems.

---

### 6. Create RabbitMQ Consumer to Consume JSON Message

**Objective:** Deserialize JSON into POJO and process business logic.

**Implementation Steps:**

- Used @RabbitListener(queues = "queue_name").
- Deserialized JSON into POJO with Jackson:
```java
Order order = objectMapper.readValue(message, Order.class);
```
- Implemented business logic like persistence or further processing.
- Added logging and exception handling.

**💡 Best Practice:** Ensures observability, resilience, and type-safe message handling.

---

## 📝 Summary

This project demonstrates how to:

- ✅ Build a Spring Boot 3 app with RabbitMQ integration.
- ✅ Publish and consume plain text and JSON messages.
- ✅ Create REST APIs for message publishing.
- ✅ Ensure decoupling, scalability, and structured communication in distributed systems.

It provides a strong foundation for enterprise-grade event-driven architectures.

## Configure RabbitMQ: 
Update the connection details in src/main/resources/application.properties:
```properties
spring.rabbitmq.host=localhost
spring.rabbitmq.port=5672
spring.rabbitmq.username=guest
spring.rabbitmq.password=guest
```

## 📚 Project Structure
- config/: RabbitMQ configuration (exchanges, queues, bindings)
- controller/: REST endpoints for message publishing
- dto/: Data Transfer Objects (DTOs) for message payloads
- publisher/: RabbitMQ message producers
- consumer/: Message consumers and handlers

## 📡 API Endpoints
Publish Text Message
```
POST /api/publish
Content-Type: text/plain

Hello, RabbitMQ!
```
Publish JSON Message
```
POST /api/publish/json
Content-Type: application/json

{
  "id": 1,
  "firstName": "John",
  "lastName": "Doe",
  "email": "john.doe@example.com"
}
```

## 🔧 Key Components
**1. RabbitMQ Configuration**
- Configures DirectExchange, Queue, and Binding as Spring @Beans
- Enables message routing and delivery
**2. Message Producers**
- RabbitMQProducer: Handles basic text message publishing
- RabbitMQJsonProducer: Manages JSON message serialization and publishing
**3. Message Consumers**
- RabbitMQConsumer: Processes incoming text messages
- RabbitMQJsonConsumer: Handles and deserializes JSON messages
**4. REST Controllers**
- MessageController: Exposes endpoints for text message publishing
- MessageJsonController: Provides API for JSON message submission

## 🚀 How to Run

1. Start RabbitMQ locally (default port 5672).

2. Clone repo:
```bash
git clone https://github.com/Sangramjit786/springboot-rabbitmq-tutorial.git
cd springboot-rabbitmq-tutorial
```

3. Run the Spring Boot app from IntelliJ or command line:
```bash
mvn spring-boot:run
```

4. Test API:

- Send text:
```bash
curl -X POST http://localhost:8080/api/publish -d "Hello RabbitMQ"
```

- Send JSON:
```bash
curl -X POST http://localhost:8080/api/publishJson -H "Content-Type: application/json" -d '{"id":1,"name":"Test Order"}'
```

## 🧪 Testing
Run the unit tests:

```bash
mvn test
```

## 📚 Tech Stack

- Spring Boot 3
- Spring Web
- Spring AMQP
- RabbitMQ
- Maven
- Java 21+

## 📦 Dependencies
- Spring Boot Starter Web
- Spring Boot Starter AMQP
- Lombok (for reducing boilerplate)
- Jackson (for JSON processing)
