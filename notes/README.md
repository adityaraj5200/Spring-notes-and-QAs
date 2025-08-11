## Spring & Spring Boot — Interview-Focused Notes

These are concise, example-driven notes to rapidly revise core Spring and Spring Boot concepts for technical interviews.

### Table of Contents
- [1. Spring Overview & Core Features](#sec-1)
- [2. Dependency Injection & Inversion of Control (IoC)](#sec-2)
- [3. Bean Lifecycle & Scopes](#sec-3)
- [4. Spring Annotations (Common & Advanced)](#sec-4)
- [5. Spring Boot Basics & Auto-Configuration](#sec-5)
- [6. Spring MVC](#sec-6)
- [7. REST APIs with Spring Boot](#sec-7)
- [8. Data Access with Spring Data JPA](#sec-8)
- [9. Spring Security Basics](#sec-9)
- [10. AOP (Aspect-Oriented Programming)](#sec-10)
- [11. Transaction Management](#sec-11)
- [12. Caching in Spring](#sec-12)
- [13. Profiles & Environment Configuration](#sec-13)
- [14. Testing in Spring](#sec-14)
- [15. Common Best Practices in Production](#sec-15)

<a id="sec-1"></a>
### 1. Spring Overview & Core Features
- **Definition:** Spring is a Java application framework offering DI/IoC, MVC, AOP, data access, security, and testing support.
- **Detailed Explanation:** Spring abstracts infrastructure concerns (object creation, lifecycle, cross-cutting concerns) so you focus on business logic. Core modules include `spring-core`, `spring-beans`, `spring-context`, `spring-aop`, with ecosystem projects like Spring Boot, Data, Security, Cloud.
- **Example:** Minimal context with component scan.
```java
@Configuration
@ComponentScan
public class AppConfig {}

public static void main(String[] args) {
    AnnotationConfigApplicationContext ctx = new AnnotationConfigApplicationContext(AppConfig.class);
    // retrieve beans, call services
    ctx.close();
}
```
- **Best Practices:** Prefer constructor injection; modularize configuration; keep controllers thin, services cohesive, repositories focused.
- **Common Pitfalls:** Overusing shared state singletons; mixing web and data responsibilities; unclear package structure breaking component scanning.
- Further reading: Spring Framework Reference `https://docs.spring.io/spring-framework/reference/`

<a id="sec-2"></a>
### 2. Dependency Injection & Inversion of Control (IoC)
- **Definition:** DI supplies object dependencies from outside; IoC delegates object creation/wiring to the container.
- **Detailed Explanation:** Spring manages bean graphs via `ApplicationContext`. Injection types: constructor (preferred), setter, field (discouraged). Qualifiers resolve multiple candidates; primary bean chosen if no qualifier.
- **Example:** Constructor injection with qualifier.
```java
public interface Notifier { void send(String msg); }

@Component("emailNotifier")
class EmailNotifier implements Notifier { public void send(String m){ /*...*/ } }

@Component("smsNotifier")
class SmsNotifier implements Notifier { public void send(String m){ /*...*/ } }

@Service
class AlertService {
  private final Notifier notifier;
  public AlertService(@Qualifier("emailNotifier") Notifier notifier) { this.notifier = notifier; }
}
```
- **Best Practices:** Use constructor injection; mark dependencies `final`; avoid optional circular dependencies; prefer interfaces for pluggability.
- **Common Pitfalls:** Field injection complicates testing; ambiguous beans without `@Qualifier`; circular dependencies.
- Further reading: Beans & DI `https://docs.spring.io/spring-framework/reference/core/beans/`

<a id="sec-3"></a>
### 3. Bean Lifecycle & Scopes
- **Definition:** Lifecycle is creation → dependency injection → initialization → ready → destruction; scope defines bean instance lifespan.
- **Detailed Explanation:** Initialization via `@PostConstruct` or `InitializingBean`; destruction via `@PreDestroy` or `DisposableBean`. Scopes: `singleton` (default), `prototype`, `request`, `session`, `application` (web).
- **Example:** Init and destroy hooks.
```java
@Component
@Scope("singleton")
class CacheManager {
  @PostConstruct void init(){ /* warm up */ }
  @PreDestroy void shutdown(){ /* flush */ }
}
```
- **Best Practices:** Keep initialization idempotent and fast; for `prototype`, manage destruction yourself; use scoped proxies for web scopes.
- **Common Pitfalls:** Relying on `@PreDestroy` for `prototype`; heavy work in constructors; forgetting thread-safety for singletons.
- Further reading: Bean scopes & lifecycle `https://docs.spring.io/spring-framework/reference/core/beans/factory-scopes.html`

<a id="sec-4"></a>
### 4. Spring Annotations (Common & Advanced)
- **Definition:** Annotations drive configuration and behavior across context, web, data, security, and AOP.
- **Detailed Explanation:** Common: `@Component`, `@Service`, `@Repository`, `@Controller`, `@RestController`, `@Configuration`, `@Bean`, `@Autowired`, `@Qualifier`, `@Primary`, `@Value`. Advanced: `@Conditional`, `@Profile`, `@ConfigurationProperties`, `@Enable*` annotations, meta-annotations, composed annotations.
- **Example:** `@ConfigurationProperties` with validation.
```java
@ConfigurationProperties(prefix = "app.mail")
@Validated
public record MailProps(@NotBlank String host, int port) {}

@EnableConfigurationProperties(MailProps.class)
@Configuration class MailConfig {}
```
- **Best Practices:** Prefer `@ConfigurationProperties` over `@Value` for groups; use meta-annotations to compose stereotypes; prefer `@Repository` for data exception translation.
- **Common Pitfalls:** Overusing `@Value`; missing `@EnableConfigurationProperties`; ambiguous component scanning due to package structure.
- Further reading: Core annotations `https://docs.spring.io/spring-framework/reference/core/beans/annotation-config.html` and Boot config props `https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#features.external-config.typesafe-configuration-properties`

<a id="sec-5"></a>
### 5. Spring Boot Basics & Auto-Configuration
- **Definition:** Boot accelerates development via auto-configuration, starters, and opinionated defaults.
- **Detailed Explanation:** Boot scans classpath, environment, and bean definitions to conditionally configure beans using `@Conditional*`. Starters aggregate dependencies; Actuator exposes operational endpoints.
- **Example:** Minimal application.
```java
@SpringBootApplication
public class Application { public static void main(String[] args){ SpringApplication.run(Application.class, args); }}
```
- **Best Practices:** Keep `@SpringBootApplication` at a root package; use Actuator in all environments; externalize config.
- **Common Pitfalls:** Classpath conflicts causing unexpected auto-config; ignoring `spring-boot-starter-validation` for `@Valid`.
- Further reading: Spring Boot Reference `https://docs.spring.io/spring-boot/docs/current/reference/html/`

<a id="sec-6"></a>
### 6. Spring MVC
- **Definition:** MVC provides a request/response web framework with controllers, view resolvers, and handler mappings.
- **Detailed Explanation:** DispatcherServlet routes requests → handler mappings → controller → view rendering or message conversion. Validation via `@Valid` and `BindingResult`.
- **Example:** Controller with validation and exception handler.
```java
record UserReq(@NotBlank String name) {}

@Controller
class UserController {
  @PostMapping("/users")
  public String create(@Valid @ModelAttribute UserReq req, BindingResult br){
    if (br.hasErrors()) return "form";
    return "redirect:/users";
  }
}

@ControllerAdvice
class GlobalErrors {
  @ExceptionHandler(Exception.class)
  public ResponseEntity<String> onError(Exception e){ return ResponseEntity.status(500).body("oops"); }
}
```
- **Best Practices:** Return `ResponseEntity` for REST; centralize errors with `@ControllerAdvice`; validate inputs.
- **Common Pitfalls:** Mixing `@Controller` and REST semantics; not handling `MethodArgumentNotValidException`.
- Further reading: Web MVC `https://docs.spring.io/spring-framework/reference/web/webmvc/`

<a id="sec-7"></a>
### 7. REST APIs with Spring Boot
- **Definition:** Build JSON/HTTP services using `@RestController`, HTTP message converters, and Boot auto-config.
- **Detailed Explanation:** Method mappings support content negotiation; `HttpMessageConverter` handles serialization; use validation and exception handling for robust APIs.
- **Example:** REST controller with DTO and validation.
```java
record CreateTodoReq(@NotBlank String title) {}
record TodoDto(Long id, String title) {}

@RestController
@RequestMapping("/api/todos")
class TodoController {
  private final TodoService service;
  TodoController(TodoService s){ this.service = s; }

  @PostMapping
  public ResponseEntity<TodoDto> create(@Valid @RequestBody CreateTodoReq req){
    TodoDto saved = service.create(req);
    return ResponseEntity.status(HttpStatus.CREATED).body(saved);
  }
}
```
- **Best Practices:** Use explicit status codes; include validation; version your API; document with OpenAPI/Swagger.
- **Common Pitfalls:** Leaking entities directly; inconsistent error format; ignoring idempotency for PUT.
- Further reading: REST with Spring `https://docs.spring.io/spring-framework/reference/web/webmvc/mvc-controller/ann-methods.html`

<a id="sec-8"></a>
### 8. Data Access with Spring Data JPA
- **Definition:** Abstraction over JPA providing repository interfaces, derived queries, and auditing.
- **Detailed Explanation:** `JpaRepository` provides CRUD; query derivation by method names; JPQL/Native queries via `@Query`; paging and sorting; entity mapping via JPA annotations.
- **Example:** Entity and repository.
```java
@Entity
class Todo {
  @Id @GeneratedValue Long id;
  String title;
  boolean done;
}

interface TodoRepo extends JpaRepository<Todo, Long> {
  List<Todo> findByDoneFalseOrderByIdDesc();
}
```
- **Best Practices:** Use DTO projections; avoid N+1 with `@EntityGraph`/fetch joins; prefer optimistic locking.
- **Common Pitfalls:** Bi-directional relationships without `mappedBy`; EAGER fetching explosion; missing transactions around writes.
- Further reading: Spring Data JPA `https://docs.spring.io/spring-data/jpa/docs/current/reference/html/`

<a id="sec-9"></a>
### 9. Spring Security Basics
- **Definition:** Provides authentication, authorization, and protection against common attacks.
- **Detailed Explanation:** Security filter chain processes requests; configure via `SecurityFilterChain` bean; password storage with `PasswordEncoder`. Method security via `@PreAuthorize`.
- **Example:** Minimal security configuration (stateless API with JWT placeholder).
```java
@Configuration
class SecurityConfig {
  @Bean SecurityFilterChain api(HttpSecurity http) throws Exception {
    http.csrf(csrf -> csrf.disable())
        .authorizeHttpRequests(auth -> auth
          .requestMatchers("/actuator/health").permitAll()
          .anyRequest().authenticated())
        .httpBasic(Customizer.withDefaults());
    return http.build();
  }
  @Bean PasswordEncoder encoder(){ return new BCryptPasswordEncoder(); }
}
```
- **Best Practices:** Use strong encoders; minimize custom filters; prefer stateless JWT for APIs; enable method security.
- **Common Pitfalls:** Disabling CSRF on session-based web apps; storing raw passwords; overly broad permitAll.
- Further reading: Spring Security Reference `https://docs.spring.io/spring-security/reference/`

<a id="sec-10"></a>
### 10. AOP (Aspect-Oriented Programming)
- **Definition:** Modularizes cross-cutting concerns like logging, security, transactions via proxies.
- **Detailed Explanation:** Spring uses proxy-based AOP (JDK/CGLIB). Join points are method executions; define pointcuts and advice types (before/after/around/after-throwing/after-returning).
- **Example:** Logging aspect.
```java
@Aspect
@Component
class LogAspect {
  @Around("execution(* com.example..service..*(..))")
  public Object around(ProceedingJoinPoint pjp) throws Throwable {
    long t0 = System.currentTimeMillis();
    try { return pjp.proceed(); }
    finally { System.out.println(pjp.getSignature()+" took "+(System.currentTimeMillis()-t0)+"ms"); }
  }
}
```
- **Best Practices:** Keep pointcuts narrow; avoid aspects on internal methods (self-invocation); prefer annotations for clarity.
- **Common Pitfalls:** Final methods not advised; internal calls bypass proxies; heavy work in advice.
- Further reading: AOP Guide `https://docs.spring.io/spring-framework/reference/core/aop.html`

<a id="sec-11"></a>
### 11. Transaction Management
- **Definition:** Ensures a set of DB operations execute atomically with ACID properties.
- **Detailed Explanation:** Declarative transactions via `@Transactional` on public methods of proxied beans. Attributes: propagation, isolation, readOnly, timeout, rollback rules.
- **Example:** Service method with propagation.
```java
@Service
class OrderService {
  @Transactional
  public void placeOrder(OrderReq req){ /* save order, items */ }

  @Transactional(propagation = Propagation.REQUIRES_NEW)
  public void audit(String msg){ /* save audit */ }
}
```
- **Best Practices:** Keep transactional boundaries at service layer; keep methods small; prefer `readOnly=true` for reads; handle checked exceptions if needed for rollback.
- **Common Pitfalls:** `@Transactional` on private/internal methods; self-invocation; forgetting to wrap write operations.
- Further reading: Transactions `https://docs.spring.io/spring-framework/reference/data-access/transaction/`

<a id="sec-12"></a>
### 12. Caching in Spring
- **Definition:** Improves performance by storing results of expensive operations.
- **Detailed Explanation:** Enable caching with `@EnableCaching`. Use `@Cacheable`, `@CachePut`, `@CacheEvict`. Backends include simple map, Caffeine, Redis, etc.
- **Example:** Cacheable service.
```java
@Service
class PriceService {
  @Cacheable(cacheNames = "prices", key = "#sku")
  public BigDecimal findPrice(String sku){ /* slow call */ return BigDecimal.TEN; }
}
```
- **Best Practices:** Define proper keys and TTLs; clear caches on updates; monitor hit ratios.
- **Common Pitfalls:** Caching mutable results; forgetting eviction on writes; cache stampede without TTL/jitter.
- Further reading: Cache Abstraction `https://docs.spring.io/spring-framework/reference/integration/cache.html`

<a id="sec-13"></a>
### 13. Profiles & Environment Configuration
- **Definition:** Profiles switch configurations/beans per environment; externalized configuration manages settings outside code.
- **Detailed Explanation:** Activate via `spring.profiles.active`. Use `@Profile` on beans and `application-{profile}.properties`. Type-safe config via `@ConfigurationProperties`.
- **Example:** Profile-specific bean.
```java
@Configuration
class DatasourceConfig {
  @Bean @Profile("dev") DataSource h2(){ return new org.h2.jdbcx.JdbcDataSource(); }
  @Bean @Profile("prod") DataSource postgres(){ /* create */ return ds; }
}
```
- **Best Practices:** Keep secrets outside properties; prefer environment variables/secret stores; validate config at startup.
- **Common Pitfalls:** Profile mismatches; hardcoding env-specific logic in code; scattering `@Value` across classes.
- Further reading: Externalized Config (Boot) `https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#features.external-config`

<a id="sec-14"></a>
### 14. Testing in Spring
- **Definition:** Spring supports unit, slice, and integration testing with context loading utilities.
- **Detailed Explanation:** `@SpringBootTest` for full context; slices like `@WebMvcTest`, `@DataJpaTest` load partial beans. Use `MockMvc`, `TestEntityManager`, `@MockBean`.
- **Example:** Controller slice test.
```java
@WebMvcTest(TodoController.class)
class TodoControllerTest {
  @Autowired MockMvc mvc;
  @MockBean TodoService service;

  @Test void create_returns201() throws Exception {
    when(service.create(any())).thenReturn(new TodoDto(1L, "a"));
    mvc.perform(post("/api/todos").contentType(MediaType.APPLICATION_JSON)
      .content("{\"title\":\"a\"}"))
      .andExpect(status().isCreated());
  }
}
```
- **Best Practices:** Prefer slice tests for speed; use Testcontainers for DB integration; avoid `@DirtiesContext` unless necessary.
- **Common Pitfalls:** Overusing full-context tests; flaky tests due to shared state; blocking calls in async tests.
- Further reading: Testing (Boot) `https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#features.testing`

<a id="sec-15"></a>
### 15. Common Best Practices in Production
- **Definition:** Guidelines to improve robustness, performance, and operability of Spring apps.
- **Detailed Explanation:** Focus on observability (logs/metrics/traces), resilience (timeouts/retries/circuit breakers), security hardening, configuration hygiene, and resource management.
- **Example:** Resilience with Spring Retry.
```java
@EnableRetry
@Service
class ClientService {
  @Retryable(maxAttempts = 3, backoff = @Backoff(delay = 200))
  public String call(){ /* remote call */ return "ok"; }
}
```
- **Best Practices:** Enable Actuator; expose health/metrics; set timeouts; validate configuration at startup; fail fast; containerize and tune JVM; use structured logging.
- **Common Pitfalls:** Unbounded thread pools; missing timeouts; ignoring backpressure; logging sensitive data.
- Further reading: Actuator `https://docs.spring.io/spring-boot/docs/current/reference/html/actuator.html` and Micrometer `https://micrometer.io/docs`


