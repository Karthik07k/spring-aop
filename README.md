# Spring Boot AOP - Aspect-Oriented Programming

This repository demonstrates how to use **Spring AOP (Aspect-Oriented Programming)** to handle cross-cutting concerns such as logging, execution time tracking, and exception handling in a **Spring Boot** application.

## What is AOP?
**Aspect-Oriented Programming (AOP)** is a programming paradigm that helps in separating cross-cutting concerns from business logic. It enables modularization of concerns like:
- Logging
- Security
- Transaction Management
- Performance Monitoring

Using AOP, we can write reusable code that applies to multiple components without modifying their actual implementation.

## Technologies Used
- Java 17+
- Spring Boot
- Spring AOP (AspectJ)
- Maven
- REST API (Spring Web)

## Key AOP Concepts
### 1. Aspect
An **Aspect** is a class that encapsulates cross-cutting logic (e.g., logging, security).

### 2. Advice
Defines what should be done and when it should run. Types:
- **`@Before`** → Runs before a method executes.
- **`@After`** → Runs after a method executes.
- **`@AfterReturning`** → Runs only if a method successfully returns a value.
- **`@AfterThrowing`** → Runs if a method throws an exception.
- **`@Around`** → Controls method execution, allowing modification of the return value.

### 3. JoinPoint
A **JoinPoint** represents a method execution where advice can be applied.

### 4. Pointcut
A **Pointcut** defines which methods should be intercepted.

## Project Structure
```
spring-aop-demo/
│── src/
│   ├── main/java/com/example/aopdemo/
│   │   ├── aspect/LoggingAspect.java
│   │   ├── aspect/ExecutionTimeAspect.java
│   │   ├── controller/UserController.java
│   │   ├── service/UserService.java
│   │   ├── AopDemoApplication.java
│── pom.xml
│── README.md
```

## Getting Started
### 1. Clone the Repository
```bash
git clone https://github.com/yourusername/spring-aop-demo.git
cd spring-aop-demo
```

### 2. Add Dependency (Spring AOP)
Ensure `pom.xml` includes:
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-aop</artifactId>
</dependency>
```

### 3. Enable AOP in Spring Boot
```java
@Configuration
@EnableAspectJAutoProxy
public class AopConfig {
}
```

## Implementing AOP Features
### 1. Logging Aspect
```java
@Aspect
@Component
public class LoggingAspect {
    @Before("execution(* com.example.aopdemo.service.*.*(..))")
    public void logBeforeMethodExecution() {
        System.out.println("Executing method...");
    }
}
```

### 2. Execution Time Tracker
```java
@Aspect
@Component
public class ExecutionTimeAspect {
    @Around("execution(* com.example.aopdemo.service.*.*(..))")
    public Object measureExecutionTime(ProceedingJoinPoint joinPoint) throws Throwable {
        long start = System.currentTimeMillis();
        Object result = joinPoint.proceed();
        long end = System.currentTimeMillis();
        System.out.println("Execution time: " + (end - start) + " ms");
        return result;
    }
}
```

### 3. Business Logic - User Service
```java
@Service
public class UserService {
    public String getUser() {
        System.out.println("Fetching user...");
        return "John Doe";
    }
}
```

### 4. REST Controller
```java
@RestController
@RequestMapping("/users")
public class UserController {
    private final UserService userService;
    public UserController(UserService userService) { this.userService = userService; }
    @GetMapping
    public String fetchUser() { return userService.getUser(); }
}
```

## Running the Application
```bash
mvn spring-boot:run
```

## Testing the API
1. Open a browser or **Postman** and make a `GET` request:
   ```
   http://localhost:8080/users
   ```
2. **Console Output:**
   ```
   Executing method...
   Fetching user...
   Execution time: 10 ms
   ```

## Additional Pointcut Expressions
| Expression | Description |
|------------|------------|
| `execution(* com.example.service.UserService.getUser(..))` | Matches a specific method |
| `execution(* com.example.service.*.*(..))` | Matches all methods in the package |
| `within(com.example.service.UserService)` | Matches all methods within `UserService` |
| `@annotation(org.springframework.transaction.annotation.Transactional)` | Matches methods annotated with `@Transactional` |

## Contributing
Fork this repository and submit a pull request with improvements.

## License
This project is licensed under the **MIT License**.

## Contact
📧 **Email:** [karthikkumarks50@gmail.com](mailto:karthikkumarks50@gmail.com)  
🔗 **GitHub:** [github.com/karthik07k](https://github.com/karthik07k)  
🔗 **LinkedIn:** [linkedin.com/in/karthikkumar-m](https://linkedin.com/in/karthikkumar-m)

