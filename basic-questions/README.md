## Basic Spring & Spring Boot Questions (50 Questions)

These are fundamental questions that interviewers might ask to assess your understanding of Java frameworks and enterprise development patterns, even if Spring isn't explicitly mentioned on your resume.

### Quick Navigation
- [Q1](#q1) · [Q2](#q2) · [Q3](#q3) · [Q4](#q4) · [Q5](#q5) · [Q6](#q6) · [Q7](#q7) · [Q8](#q8) · [Q9](#q9) · [Q10](#q10)
- [Q11](#q11) · [Q12](#q12) · [Q13](#q13) · [Q14](#q14) · [Q15](#q15) · [Q16](#q16) · [Q17](#q17) · [Q18](#q18) · [Q19](#q19) · [Q20](#q20)
- [Q21](#q21) · [Q22](#q22) · [Q23](#q23) · [Q24](#q24) · [Q25](#q25) · [Q26](#q26) · [Q27](#q27) · [Q28](#q28) · [Q29](#q29) · [Q30](#q30)
- [Q31](#q31) · [Q32](#q32) · [Q33](#q33) · [Q34](#q34) · [Q35](#q35) · [Q36](#q36) · [Q37](#q37) · [Q38](#q38) · [Q39](#q39) · [Q40](#q40)
- [Q41](#q41) · [Q42](#q42) · [Q43](#q43) · [Q44](#q44) · [Q45](#q45) · [Q46](#q46) · [Q47](#q47) · [Q48](#q48) · [Q49](#q49) · [Q50](#q50)

---

### Q1: What is Dependency Injection?
**Answer:** A design pattern where a class receives its dependencies from an external source rather than creating them itself. This promotes loose coupling, testability, and modularity.

**Example:**
```java
@Service
class UserService {
    private final UserRepository repository;
    
    // Constructor injection - dependencies provided externally
    UserService(UserRepository repository) {
        this.repository = repository;
    }
}
```

---

### Q2: What is Inversion of Control (IoC)?
**Answer:** A principle where the control of object creation and dependency management is inverted - instead of your code controlling object creation, a framework or container manages it. This enables features like dependency injection and aspect-oriented programming.

---

### Q3: What is a Bean in Spring?
**Answer:** A bean is an object that is managed by the Spring IoC container. Spring creates, configures, and manages the lifecycle of these objects. Beans can be defined using annotations like `@Component`, `@Service`, or `@Bean` methods.

---

### Q4: What is the difference between @Component, @Service, and @Repository?
**Answer:** All three are specializations of `@Component`:
- `@Component`: Generic stereotype for any Spring-managed component
- `@Service`: Indicates a service layer component (business logic)
- `@Repository`: Indicates a data access component with exception translation

---

### Q5: What is @Autowired annotation?
**Answer:** `@Autowired` tells Spring to inject a dependency automatically. It can be used on constructors, setters, or fields. Spring will find a bean of the required type and inject it.

**Example:**
```java
@Service
class EmailService {
    @Autowired
    private EmailSender emailSender;
}
```

---

### Q6: What is constructor injection and why prefer it?
**Answer:** Constructor injection means dependencies are provided through the constructor. It's preferred because it ensures required dependencies are provided, makes the class immutable, and is easier to test.

**Example:**
```java
@Service
class PaymentService {
    private final PaymentGateway gateway;
    private final Logger logger;
    
    PaymentService(PaymentGateway gateway, Logger logger) {
        this.gateway = gateway;
        this.logger = logger;
    }
}
```

---

### Q7: What are Spring profiles?
**Answer:** Profiles allow you to configure different beans or properties for different environments (dev, test, prod). You can activate profiles using `spring.profiles.active` property.

**Example:**
```java
@Configuration
@Profile("dev")
class DevConfig {
    @Bean
    DataSource dataSource() {
        return new H2DataSource();
    }
}
```

---

### Q8: What is @Value annotation?
**Answer:** `@Value` injects values from properties files, environment variables, or default values directly into fields.

**Example:**
```java
@Component
class AppConfig {
    @Value("${app.name:DefaultApp}")
    private String appName;
    
    @Value("${server.port:8080}")
    private int serverPort;
}
```

---

### Q9: What is @ConfigurationProperties?
**Answer:** Binds external configuration to a Java object, providing type-safe configuration with validation support.

**Example:**
```java
@ConfigurationProperties(prefix = "app.mail")
record MailConfig(String host, int port, String username) {}
```

---

### Q10: What is @RestController?
**Answer:** A convenience annotation that combines `@Controller` and `@ResponseBody`. It's used for REST APIs where methods return data that should be written directly to the HTTP response body.

<details>
<summary>What do `@ResponseBody` do?</summary>

`@ResponseBody` in Spring tells Spring MVC:

> “Don’t render a view — instead, take the method’s return value, convert it to a suitable format (usually JSON or XML), and write it directly to the HTTP response body.”

---

## **1️⃣ Without `@ResponseBody`**

By default, Spring MVC assumes the method returns a **view name** (like `index`, `home`, etc.).

```java
@Controller
public class MyController {

    @GetMapping("/hello")
    public String hello() {
        return "hello"; // Looks for hello.html or hello.jsp
    }
}
```

---

## **2️⃣ With `@ResponseBody`**

Now Spring will:

1. Take the return value.
2. Pass it to an **HttpMessageConverter** (e.g., Jackson for JSON).
3. Write the result directly to the HTTP response body.

```java
@Controller
public class MyController {

    @GetMapping("/hello")
    @ResponseBody
    public String hello() {
        return "Hello, World!"; // Sends plain text as response
    }
}
```

📤 Output in browser:

```
Hello, World!
```

(No view rendering, no HTML template lookup.)

---

## **3️⃣ Common use case: JSON APIs**

```java
@Controller
public class UserController {

    @GetMapping("/user")
    @ResponseBody
    public User getUser() {
        return new User("Alice", 25); // Will be JSON
    }
}
```

📤 Output:

```json
{"name":"Alice","age":25}
```

*(Assuming Jackson is on the classpath)*

---

## **4️⃣ Shortcut: `@RestController`**

Instead of writing `@Controller` + `@ResponseBody` on every method,
use `@RestController` → it **implies `@ResponseBody` for all methods**.

```java
@RestController
public class UserController {
    @GetMapping("/user")
    public User getUser() { ... } // auto JSON
}
```

---

✅ **TL;DR:**
`@ResponseBody` bypasses view rendering and sends your method’s return value straight into the HTTP response body, usually after converting it to JSON or XML.

---


</details> 

---

### Q11: What is @RequestMapping?
**Answer:** Maps web requests to handler methods. It can be applied at class or method level to specify URL patterns, HTTP methods, and other request attributes.

**Example:**
```java
@RestController
@RequestMapping("/api/users")
class UserController {
    @GetMapping("/{id}")
    User getUser(@PathVariable Long id) { /* ... */ }
}
```

---

### Q12: What is @PathVariable?
**Answer:** Binds a method parameter to a URI template variable. It extracts values from the URL path.

**Example:**
```java
@GetMapping("/users/{id}/orders/{orderId}")
Order getOrder(@PathVariable Long id, @PathVariable Long orderId) { /* ... */ }
```

---

### Q13: What is @RequestParam?
**Answer:** Binds method parameters to query parameters, form data, or parts of multipart requests.

**Example:**
```java
@GetMapping("/users")
List<User> getUsers(@RequestParam(defaultValue = "0") int page, 
                    @RequestParam(defaultValue = "10") int size) { /* ... */ }
```

---

### Q14: What is @RequestBody?
**Answer:** Binds the HTTP request body to a method parameter. It's commonly used with POST/PUT requests to receive JSON or XML data.

**Example:**
```java
@PostMapping("/users")
User createUser(@RequestBody CreateUserRequest request) { /* ... */ }
```

---

### Q15: What is @Valid annotation?
**Answer:** Triggers validation of the annotated object using Bean Validation annotations like `@NotNull`, `@Size`, etc.

**Example:**
```java
@PostMapping("/users")
User createUser(@Valid @RequestBody CreateUserRequest request) { /* ... */ }

record CreateUserRequest(@NotBlank String name, @Email String email) {}
```

---

### Q16: What is @ExceptionHandler?
**Answer:** Defines a method to handle exceptions thrown by controller methods. It can be used in `@ControllerAdvice` for global exception handling.

**Example:**
```java
@ControllerAdvice
class GlobalExceptionHandler {
    @ExceptionHandler(UserNotFoundException.class)
    ResponseEntity<ErrorResponse> handleUserNotFound(UserNotFoundException ex) {
        return ResponseEntity.status(404).body(new ErrorResponse("User not found"));
    }
}
```

---

### Q17: What is @Transactional?
**Answer:** Declares that a method should run within a database transaction. It provides ACID properties and can be configured with propagation, isolation, and rollback rules.

**Example:**
```java
@Transactional
public void transferMoney(Account from, Account to, BigDecimal amount) {
    from.debit(amount);
    to.credit(amount);
    accountRepository.save(from);
    accountRepository.save(to);
}
```

---

### Q18: What is @Repository?
**Answer:** A stereotype annotation that indicates a class is a data access object. It enables exception translation from database-specific exceptions to Spring's `DataAccessException` hierarchy.

---

### Q19: What is Spring Data JPA?
**Answer:** A Spring module that simplifies data access by providing repository abstractions, query methods, and common data access patterns without writing boilerplate code.

**Example:**
```java
@Repository
interface UserRepository extends JpaRepository<User, Long> {
    List<User> findByEmailContaining(String email);
    Optional<User> findByUsername(String username);
}
```

---

### Q20: What is @Entity?
**Answer:** Marks a class as a JPA entity that maps to a database table. It's used with JPA annotations like `@Id`, `@Column`, and `@Table`.

**Example:**
```java
@Entity
@Table(name = "users")
class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @Column(nullable = false)
    private String username;
}
```

---

### Q21: What is @Id annotation?
**Answer:** Marks a field as the primary key of an entity. It's required for every JPA entity.

---

### Q22: What is @GeneratedValue?
**Answer:** Specifies how the primary key value should be generated automatically (e.g., auto-increment, sequence, table).

**Example:**
```java
@Id
@GeneratedValue(strategy = GenerationType.IDENTITY)
private Long id;
```

---

### Q23: What is lazy loading vs eager loading?
**Answer:** 
- **Lazy loading**: Associations are loaded only when accessed (default for collections)
- **Eager loading**: Associations are loaded immediately with the parent entity

**Example:**
```java
@OneToMany(fetch = FetchType.LAZY) // Load only when accessed
private List<Order> orders;

@OneToOne(fetch = FetchType.EAGER) // Load immediately
private Address address;
```

---

### Q24: What is N+1 query problem?
**Answer:** A performance issue where fetching a list of entities results in 1 query for the list plus N queries for each entity's associations. It can be solved using fetch joins, `@EntityGraph`, or DTO projections.

---

### Q25: What is @Query annotation?
**Answer:** Allows you to define custom JPQL or native SQL queries for repository methods.

**Example:**
```java
@Query("SELECT u FROM User u WHERE u.active = true")
List<User> findActiveUsers();

@Query(value = "SELECT * FROM users WHERE created_date > ?1", nativeQuery = true)
List<User> findUsersCreatedAfter(Date date);
```

---

### Q26: What is @EnableCaching?
**Answer:** Enables Spring's caching abstraction, allowing you to use caching annotations like `@Cacheable`, `@CacheEvict`, and `@CachePut`.

---

### Q27: What is @Cacheable?
**Answer:** Caches the result of a method invocation. Subsequent calls with the same parameters return the cached result instead of executing the method.

**Example:**
```java
@Cacheable("users")
public User findById(Long id) {
    return userRepository.findById(id);
}
```

---

### Q28: What is Spring Security?
**Answer:** A framework that provides authentication, authorization, and protection against common security vulnerabilities like CSRF, XSS, and session fixation.

---

### Q29: What is @EnableWebSecurity?
**Answer:** Enables Spring Security's web security support and creates a `SecurityFilterChain` bean that can be customized.

---

### Q30: What is @PreAuthorize?
**Answer:** Method-level security annotation that checks authorization before method execution using SpEL expressions.

**Example:**
```java
@PreAuthorize("hasRole('ADMIN') or #userId == authentication.name")
public User getUser(String userId) { /* ... */ }
```

---

### Q31: What is CSRF protection?
**Answer:** Cross-Site Request Forgery protection prevents malicious websites from making unauthorized requests on behalf of authenticated users. It's enabled by default in Spring Security for stateful applications.

---

### Q32: What is CORS?
**Answer:** Cross-Origin Resource Sharing allows web applications to make requests to different domains. It's configured to specify allowed origins, methods, and headers.

---

### Q33: What is @EnableAsync?
**Answer:** Enables asynchronous method execution using `@Async` annotation, allowing methods to run in separate threads.

**Example:**
```java
@EnableAsync
@Configuration
class AsyncConfig {}

@Service
class EmailService {
    @Async
    public CompletableFuture<String> sendEmail(String to) { /* ... */ }
}
```

---

### Q34: What is @Scheduled?
**Answer:** Executes a method at fixed intervals or using cron expressions. Requires `@EnableScheduling`.

**Example:**
```java
@EnableScheduling
@Configuration
class SchedulingConfig {}

@Component
class ReportGenerator {
    @Scheduled(fixedRate = 60000) // Every minute
    public void generateReport() { /* ... */ }
}
```

---

### Q35: What is @ConditionalOnProperty?
**Answer:** Conditionally creates a bean based on the presence and value of a configuration property.

**Example:**
```java
@Bean
@ConditionalOnProperty(name = "feature.email.enabled", havingValue = "true")
EmailService emailService() {
    return new EmailService();
}
```

---

### Q36: What is @ConditionalOnClass?
**Answer:** Conditionally creates a bean only if a specified class is present on the classpath.

**Example:**
```java
@Bean
@ConditionalOnClass(name = "com.example.ExternalService")
ExternalServiceClient externalServiceClient() {
    return new ExternalServiceClient();
}
```

---

### Q37: What is @EnableConfigurationProperties?
**Answer:** Enables binding of `@ConfigurationProperties` classes to external configuration.

**Example:**
```java
@EnableConfigurationProperties(MailConfig.class)
@Configuration
class MailConfiguration {}
```

---

### Q38: What is @Configuration?
**Answer:** Indicates that a class contains bean definitions. It's processed by Spring to create and configure beans.

---

### Q39: What is @Bean annotation?
**Answer:** Indicates that a method produces a bean to be managed by the Spring container.

**Example:**
```java
@Configuration
class DatabaseConfig {
    @Bean
    DataSource dataSource() {
        return new HikariDataSource();
    }
}
```

---

### Q40: What is @Primary annotation?
**Answer:** Indicates that a bean should be given preference when multiple candidates are qualified to autowire a single-valued dependency.

---

### Q41: What is @Qualifier annotation?
**Answer:** Specifies which bean to inject when multiple beans of the same type exist.

**Example:**
```java
@Service
class PaymentService {
    private final PaymentGateway gateway;
    
    PaymentService(@Qualifier("stripeGateway") PaymentGateway gateway) {
        this.gateway = gateway;
    }
}
```

---

### Q42: What is @Scope annotation?
**Answer:** Specifies the scope of a bean (singleton, prototype, request, session, etc.).

**Example:**
```java
@Component
@Scope("prototype")
class PrototypeBean {}
```

---

### Q43: What is @PostConstruct?
**Answer:** Marks a method to be executed after dependency injection is complete. It's used for initialization logic.

**Example:**
```java
@Component
class CacheManager {
    @PostConstruct
    public void initialize() {
        // Load initial data into cache
    }
}
```

---

### Q44: What is @PreDestroy?
**Answer:** Marks a method to be executed before the bean is destroyed. It's used for cleanup logic.

**Example:**
```java
@Component
class CacheManager {
    @PreDestroy
    public void cleanup() {
        // Flush cache to disk
    }
}
```

---

### Q45: What is @ControllerAdvice?
**Answer:** A specialization of `@Component` that allows you to handle exceptions across the whole application in one global handling class.

---

### Q46: What is @ModelAttribute?
**Answer:** Binds a method parameter or method return value to a named model attribute, exposed to a web view.

---

### Q47: What is @ResponseBody?
**Answer:** Indicates that a method return value should be bound to the web response body.

---

### Q48: What is @ResponseStatus?
**Answer:** Maps an exception or controller method to an HTTP status code.

**Example:**
```java
@ResponseStatus(HttpStatus.CREATED)
public void createUser() { /* ... */ }
```

---

### Q49: What is @EnableJpaRepositories?
**Answer:** Enables JPA repositories and configures the base packages to scan for repository interfaces.

---

### Q50: What is @EnableTransactionManagement?
**Answer:** Enables Spring's annotation-driven transaction management capability, allowing the use of `@Transactional` annotation.

---

### Study Tips
- Focus on understanding the concepts rather than memorizing syntax
- Practice explaining these concepts in simple terms
- Be ready to provide real-world examples of when you'd use each feature
- Understand the benefits and trade-offs of different approaches
