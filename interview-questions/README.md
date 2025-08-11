## Top 100 Spring & Spring Boot Interview Questions and Answers

This set is grouped into Basic (Q1–Q50) and Advanced (Q51–Q100). Each question has a short, precise answer and occasional example snippets.

### Quick Links
- [Q1](#q1) · [Q2](#q2) · [Q3](#q3) · [Q4](#q4) · [Q5](#q5) · [Q6](#q6) · [Q7](#q7) · [Q8](#q8) · [Q9](#q9) · [Q10](#q10)
- [Q11](#q11) · [Q12](#q12) · [Q13](#q13) · [Q14](#q14) · [Q15](#q15) · [Q16](#q16) · [Q17](#q17) · [Q18](#q18) · [Q19](#q19) · [Q20](#q20)
- [Q21](#q21) · [Q22](#q22) · [Q23](#q23) · [Q24](#q24) · [Q25](#q25) · [Q26](#q26) · [Q27](#q27) · [Q28](#q28) · [Q29](#q29) · [Q30](#q30)
- [Q31](#q31) · [Q32](#q32) · [Q33](#q33) · [Q34](#q34) · [Q35](#q35) · [Q36](#q36) · [Q37](#q37) · [Q38](#q38) · [Q39](#q39) · [Q40](#q40)
- [Q41](#q41) · [Q42](#q42) · [Q43](#q43) · [Q44](#q44) · [Q45](#q45) · [Q46](#q46) · [Q47](#q47) · [Q48](#q48) · [Q49](#q49) · [Q50](#q50)
- [Q51](#q51) · [Q52](#q52) · [Q53](#q53) · [Q54](#q54) · [Q55](#q55) · [Q56](#q56) · [Q57](#q57) · [Q58](#q58) · [Q59](#q59) · [Q60](#q60)
- [Q61](#q61) · [Q62](#q62) · [Q63](#q63) · [Q64](#q64) · [Q65](#q65) · [Q66](#q66) · [Q67](#q67) · [Q68](#q68) · [Q69](#q69) · [Q70](#q70)
- [Q71](#q71) · [Q72](#q72) · [Q73](#q73) · [Q74](#q74) · [Q75](#q75) · [Q76](#q76) · [Q77](#q77) · [Q78](#q78) · [Q79](#q79) · [Q80](#q80)
- [Q81](#q81) · [Q82](#q82) · [Q83](#q83) · [Q84](#q84) · [Q85](#q85) · [Q86](#q86) · [Q87](#q87) · [Q88](#q88) · [Q89](#q89) · [Q90](#q90)
- [Q91](#q91) · [Q92](#q92) · [Q93](#q93) · [Q94](#q94) · [Q95](#q95) · [Q96](#q96) · [Q97](#q97) · [Q98](#q98) · [Q99](#q99) · [Q100](#q100)

---

### Basic Level (Q1–Q50)

<a id="q1"></a>
### Q1: What is Dependency Injection (DI) in Spring?
**Answer:** Dependency Injection lets the container create and supply an object's dependencies, so classes focus on behavior rather than wiring. This improves testability (mocking), maintainability (loose coupling), and reusability. In Spring, the `ApplicationContext` resolves and provides beans based on type and qualifiers.

**Example:** Constructor injection with multiple implementations disambiguated via `@Qualifier`.
```java
public interface Notifier { void send(String message); }

@Component("emailNotifier")
class EmailNotifier implements Notifier { public void send(String m) {/*...*/} }

@Component("smsNotifier")
class SmsNotifier implements Notifier { public void send(String m) {/*...*/} }

@Service
class AlertService {
  private final Notifier notifier;
  public AlertService(@Qualifier("emailNotifier") Notifier notifier) { this.notifier = notifier; }
}
```

<a id="q2"></a>
### Q2: What is Inversion of Control (IoC)?
**Answer:** IoC means the framework takes over control of creating and managing objects. Instead of your code `new`-ing dependencies, Spring builds the object graph and injects collaborators. This enables cross-cutting services (AOP, transactions) and lifecycle management.

**Example:** Retrieving beans from the IoC container.
```java
try (var ctx = new AnnotationConfigApplicationContext(AppConfig.class)) {
  PaymentService service = ctx.getBean(PaymentService.class);
  service.charge(...);
}
```

<a id="q3"></a>
### Q3: Bean vs Component in Spring?
**Answer:** Use `@Component` (and stereotypes like `@Service`, `@Repository`) to have Spring discover classes via component scanning. Use `@Bean` within a `@Configuration` class to register bean instances produced by factory methods. Prefer `@Component` for your own classes; use `@Bean` for third-party objects or when you need explicit construction logic.

**Example:** Mixing both approaches.
```java
@Configuration
class MailConfig {
  @Bean JavaMailSender mailSender() { return new JavaMailSenderImpl(); }
}

@Service // discovered via @ComponentScan
class MailService { MailService(JavaMailSender sender) { /*...*/ } }
```

<a id="q4"></a>
### Q4: Constructor vs Setter injection?
**Answer:** Prefer constructor injection for required dependencies and immutability, making objects always valid. Use setter injection for optional or reconfigurable dependencies. Constructor injection is friendlier for tests and prevents circular dependencies from being silently tolerated.

**Example:** Required vs optional dependency.
```java
@Service
class ReportService {
  private final Repository repo; // required
  private Clock clock = Clock.systemUTC(); // optional default

  ReportService(Repository repo) { this.repo = repo; }
  @Autowired void setClock(Clock clock) { this.clock = clock; }
}
```

<a id="q5"></a>
### Q5: What are common bean scopes?
**Answer:** `singleton` (one instance per container), `prototype` (new instance on each request), and web scopes `request`, `session`, `application`. Use scoped proxies (`proxyMode = TARGET_CLASS`) when injecting web-scoped beans into singletons.

**Example:** Request-scoped bean used in a singleton.
```java
@Component
@Scope(value = WebApplicationContext.SCOPE_REQUEST, proxyMode = ScopedProxyMode.TARGET_CLASS)
class RequestContext { String requestId = UUID.randomUUID().toString(); }

@Service
class AuditService { AuditService(RequestContext ctx) { /* safe via proxy */ } }
```

<a id="q6"></a>
### Q6: Describe the bean lifecycle.
**Answer:** Spring instantiates the bean, injects dependencies, runs initialization callbacks (`@PostConstruct` or `InitializingBean.afterPropertiesSet`), then the bean is ready for use. On context shutdown, destruction callbacks (`@PreDestroy` or `DisposableBean.destroy`) run. For `prototype` scope, destruction isn’t managed.

**Example:** Initialization and cleanup hooks.
```java
@Component
class CacheManager {
  @PostConstruct void init() { /* warm up cache */ }
  @PreDestroy  void shutdown() { /* flush */ }
}
```

<a id="q7"></a>
### Q7: What is `ApplicationContext` vs `BeanFactory`?
**Answer:** `BeanFactory` is the basic container; `ApplicationContext` adds enterprise features: events, internationalization, resource loading, AOP integration, and environment abstraction. In applications, always use `ApplicationContext` (Boot provides it automatically).

<a id="q8"></a>
### Q8: What does `@Primary` do?
**Answer:** Marks a bean as the default candidate when multiple beans of the same type exist and no `@Qualifier` is provided. It’s a tie-breaker, not a replacement for explicit `@Qualifier` in complex graphs.

**Example:**
```java
@Bean @Primary Notifier email() { return new EmailNotifier(); }
@Bean Notifier sms() { return new SmsNotifier(); }
```

<a id="q9"></a>
### Q9: When do you need `@Qualifier`?
**Answer:** Whenever multiple beans of the same type exist and your injection point must select a specific one. Combine with `@Primary` thoughtfully.

**Example:**
```java
public PaymentService(@Qualifier("stripeGateway") PaymentGateway gateway) { /*...*/ }
```

<a id="q10"></a>
### Q10: `@Component` vs `@Service` vs `@Repository`?
**Answer:** All are detectable components. Use `@Service` for business logic semantics; `@Repository` adds persistence exception translation (`@Repository`-annotated beans convert JPA exceptions into Spring’s `DataAccessException` hierarchy); `@Component` is generic.

**Example:**
```java
@Repository interface UserRepo extends JpaRepository<User, Long> {}
@Service class UserService { UserService(UserRepo repo) { /*...*/ } }
```

<a id="q11"></a>
### Q11: What is auto-configuration in Spring Boot?
**Answer:** Auto-configuration uses conditional annotations (`@ConditionalOnClass`, `@ConditionalOnMissingBean`, etc.) to create sensible defaults based on the classpath, properties, and existing beans. You can exclude or override parts as needed.

**Example:** Enable debug to see why configs applied:
```
debug=true
```

<a id="q12"></a>
### Q12: What are Spring Boot starters?
**Answer:** Starters are curated dependency BOMs (e.g., `spring-boot-starter-web`, `spring-boot-starter-data-jpa`) that bring a set of libraries and compatible versions for a feature, reducing dependency management friction.

<a id="q13"></a>
### Q13: Purpose of `@SpringBootApplication`?
**Answer:** It combines `@Configuration`, `@EnableAutoConfiguration`, and `@ComponentScan`. Place it in a top-level package so component scanning covers your application packages.

**Example:**
```java
@SpringBootApplication
public class App { public static void main(String[] args){ SpringApplication.run(App.class,args);} }
```

<a id="q14"></a>
### Q14: How to change server port in Boot?
**Answer:** Set `server.port=8081` in `application.properties` or pass `--server.port=8081` as an argument. For tests, override via `@TestPropertySource`.

<a id="q15"></a>
### Q15: What is `@ConfigurationProperties`?
**Answer:** Binds hierarchical configuration to a strongly-typed bean with optional validation. Prefer it over scattering `@Value` fields.

**Example:**
```java
@ConfigurationProperties(prefix = "app.mail")
@Validated
record MailProps(@NotBlank String host, int port) {}
```

<a id="q16"></a>
### Q16: `@Value` vs `@ConfigurationProperties`?
**Answer:** Use `@Value` for simple, isolated values; use `@ConfigurationProperties` for grouped settings with metadata and validation. The latter is better for maintainability.

**Example:**
```java
@Value("${feature.enabled:false}")
boolean featureEnabled;
```

<a id="q17"></a>
### Q17: What is `@RestController`?
**Answer:** It’s a specialized `@Controller` where every handler method’s return value is written to the HTTP response body (typically JSON) via message converters.

**Example:**
```java
@RestController
@RequestMapping("/api/users")
class UserController { @GetMapping("/{id}") UserDto find(@PathVariable Long id){ return service.find(id);} }
```

<a id="q18"></a>
### Q18: How to handle validation errors in REST?
**Answer:** Annotate request DTOs with Bean Validation and use `@Valid`. Add a `@ControllerAdvice` to catch `MethodArgumentNotValidException` and return consistent error responses.

**Example:**
```java
record CreateUserReq(@NotBlank String name) {}

@ControllerAdvice
class Errors {
  @ExceptionHandler(MethodArgumentNotValidException.class)
  ResponseEntity<Map<String,Object>> handle(MethodArgumentNotValidException ex){
    Map<String,Object> body = Map.of(
      "status", 400,
      "errors", ex.getBindingResult().getFieldErrors()
                  .stream().map(f -> Map.of(f.getField(), f.getDefaultMessage())).toList());
    return ResponseEntity.badRequest().body(body);
  }
}
```

<a id="q19"></a>
### Q19: Difference between `@RequestParam` and `@PathVariable`?
**Answer:** `@PathVariable` binds parts of the URI path (e.g., `/users/{id}`), representing resource identity. `@RequestParam` binds query string/form parameters for filtering, pagination, options.

**Example:**
```java
@GetMapping("/users/{id}") UserDto get(@PathVariable Long id) { /*...*/ }
@GetMapping("/users") Page<UserDto> list(@RequestParam int page, @RequestParam int size) { /*...*/ }
```

<a id="q20"></a>
### Q20: How do `HttpMessageConverter`s work?
**Answer:** Spring chooses a converter based on the `Content-Type` of the request and the `Accept` header of the response along with the Java type. Jackson handles JSON, JAXB/Jackson XML for XML, etc. You can add custom converters.

**Example:** Customizing Jackson globally.
```java
@Bean
Jackson2ObjectMapperBuilderCustomizer json() { return b -> b.failOnUnknownProperties(false); }
```

<a id="q21"></a>
### Q21: `ResponseEntity` usage?
**Answer:** Wrap response with status, headers, and body for precise control.

<a id="q22"></a>
### Q22: How to create custom exception handling?
**Answer:** Define `@ControllerAdvice` with `@ExceptionHandler` methods.

<a id="q23"></a>
### Q23: `@Transactional` default behavior?
**Answer:** Propagation `REQUIRED`, rollback on unchecked exceptions, and applied to public methods via proxies.

<a id="q24"></a>
### Q24: Difference between `REQUIRED` and `REQUIRES_NEW`?
**Answer:** `REQUIRED` joins or creates a transaction; `REQUIRES_NEW` suspends current and starts a new one.

<a id="q25"></a>
### Q25: Isolation levels?
**Answer:** `READ_UNCOMMITTED`, `READ_COMMITTED`, `REPEATABLE_READ`, `SERIALIZABLE`—control visibility and anomalies.

<a id="q26"></a>
### Q26: `@Entity` vs DTO in REST?
**Answer:** Avoid exposing `@Entity` directly; use DTOs to control fields, validation, and decouple persistence.

<a id="q27"></a>
### Q27: `CrudRepository` vs `JpaRepository`?
**Answer:** `JpaRepository` extends `PagingAndSortingRepository` and adds JPA-specific utilities (batching, flush, etc.).

<a id="q28"></a>
### Q28: Derived query method example?
**Answer:** `List<User> findByStatusAndCreatedAtAfter(Status s, Instant from);`

<a id="q29"></a>
### Q29: Prevent N+1 in JPA?
**Answer:** Use fetch joins, `@EntityGraph`, or DTO projections.

<a id="q30"></a>
### Q30: Lazy vs Eager loading?
**Answer:** LAZY loads associations on access; EAGER loads immediately—prefer LAZY to avoid heavy queries.

<a id="q31"></a>
### Q31: What is Spring AOP?
**Answer:** Spring AOP applies cross-cutting behavior (logging, metrics, security checks, transactions) via proxies that wrap beans. Advices run at join points (method executions) matching pointcuts.

**Example:** Simple timing aspect.
```java
@Aspect @Component
class TimingAspect {
  @Around("execution(* com.example..service..*(..))")
  Object time(ProceedingJoinPoint pjp) throws Throwable {
    long t0 = System.nanoTime();
    try { return pjp.proceed(); }
    finally { System.out.println(pjp.getSignature()+" took "+(System.nanoTime()-t0)/1_000_000+"ms"); }
  }
}
```

<a id="q32"></a>
### Q32: Types of AOP advice?
**Answer:** `@Before` (pre-invocation), `@AfterReturning` (on success), `@AfterThrowing` (on exception), `@After` (finally), and `@Around` (wraps invocation, can short-circuit or modify arguments/return).

**Example:**
```java
@Before("execution(* com.example.UserService.*(..))")
void check(){ /* preconditions */ }
```

<a id="q33"></a>
### Q33: Common AOP pitfall?
**Answer:** Self-invocation bypasses proxies. When a bean method calls another method on the same bean, the call doesn’t go through the proxy, so advice (including `@Transactional`) won’t apply.

**Example:**
```java
@Service
class ReportService {
  @Transactional public void create(){ save(); } // calling own method
  @Transactional public void save(){ /* NOT proxied when called internally */ }
}
```

<a id="q34"></a>
### Q34: Spring Security filter chain?
**Answer:** A sequence of filters handles authentication, CSRF, session, authorization, etc. Configure via a `SecurityFilterChain` bean. The order matters; Boot auto-registers standard filters.

**Example:** Minimal stateless config.
```java
@Bean SecurityFilterChain api(HttpSecurity http) throws Exception {
  http.csrf(csrf -> csrf.disable())
      .sessionManagement(sm -> sm.sessionCreationPolicy(SessionCreationPolicy.STATELESS))
      .authorizeHttpRequests(a -> a.requestMatchers("/public/**").permitAll().anyRequest().authenticated())
      .httpBasic(Customizer.withDefaults());
  return http.build();
}
```

<a id="q35"></a>
### Q35: Password storage best practice?
**Answer:** Use a strong, adaptive hash like BCrypt, Argon2, or PBKDF2 via `PasswordEncoder`. Never store plain text or unsalted hashes.

**Example:**
```java
@Bean PasswordEncoder encoder(){ return new BCryptPasswordEncoder(); }
```

<a id="q36"></a>
### Q36: Enable method-level security?
**Answer:** Add `@EnableMethodSecurity` to a `@Configuration` class, then guard methods with `@PreAuthorize`, `@PostAuthorize`, or use post-filtering annotations.

**Example:**
```java
@PreAuthorize("hasRole('ADMIN') or #id == authentication.name")
public UserDto get(String id){ /*...*/ }
```

<a id="q37"></a>
### Q37: CSRF in REST APIs?
**Answer:** For stateless token-based APIs, CSRF can usually be disabled; for session-based apps, keep it enabled.

<a id="q38"></a>
### Q38: CORS configuration?
**Answer:** For browsers to call your API across origins, configure CORS with allowed origins, methods, and headers. Either define a `CorsConfigurationSource` bean or enable `http.cors()` and set properties.

**Example:**
```java
@Bean CorsConfigurationSource cors() {
  var config = new CorsConfiguration();
  config.setAllowedOrigins(List.of("https://app.example.com"));
  config.setAllowedMethods(List.of("GET","POST","PUT","DELETE"));
  config.setAllowedHeaders(List.of("*"));
  var source = new UrlBasedCorsConfigurationSource();
  source.registerCorsConfiguration("/**", config);
  return source;
}
```

<a id="q39"></a>
### Q39: How to handle exceptions globally?
**Answer:** `@ControllerAdvice` with typed `@ExceptionHandler` methods returning consistent error responses.

<a id="q40"></a>
### Q40: Difference between `@Controller` and `@RestController`?
**Answer:** `@Controller` returns views; `@RestController` returns bodies directly (JSON by default).

<a id="q41"></a>
### Q41: How to return 201 Created with Location header?
**Answer:** Build a `201 Created` response with a `Location` header pointing to the new resource URI.

**Example:**
```java
URI uri = URI.create("/api/users/" + saved.getId());
return ResponseEntity.created(uri).body(saved);
```

<a id="q42"></a>
### Q42: Purpose of Actuator?
**Answer:** Adds production-ready endpoints: health checks, metrics, HTTP trace, environment info, thread dump, etc. Integrates with Micrometer for metrics backends.

**Example:** Properties to expose health and metrics:
```
management.endpoints.web.exposure.include=health,info,metrics,prometheus
management.endpoint.health.show-details=when_authorized
```

<a id="q43"></a>
### Q43: Enable metrics export?
**Answer:** Add the appropriate Micrometer registry (e.g., Prometheus), expose `/actuator/prometheus`, and scrape from your monitoring system.

**Example:**
```
dependencies { implementation("io.micrometer:micrometer-registry-prometheus") }
management.endpoints.web.exposure.include=prometheus
```

<a id="q44"></a>
### Q44: How does property precedence work in Boot?
**Answer:** Highest to lowest: command-line args, test properties, OS env vars, `application-{profile}.properties`, `application.properties`, then defaults. Later property sources can override earlier ones.

<a id="q45"></a>
### Q45: Profiles usage?
**Answer:** Activate with `spring.profiles.active` and condition beans with `@Profile("dev")` etc.

<a id="q46"></a>
### Q46: What does `@EnableAutoConfiguration` do?
**Answer:** Triggers Boot’s conditional configuration based on classpath and environment.

<a id="q47"></a>
### Q47: Example of custom `@ConfigurationProperties`?
**Answer:** Use a dedicated type with a prefix and register it for binding. Validate where necessary.
```java
@ConfigurationProperties(prefix="app.mail")
@Validated
record MailProps(@NotBlank String host, int port) {}
```

<a id="q48"></a>
### Q48: How to disable a specific auto-configuration?
**Answer:** Exclude via annotation or properties when you want full manual control or to avoid conflicting defaults.
```java
@SpringBootApplication(exclude = DataSourceAutoConfiguration.class)
public class App { /*...*/ }
```

<a id="q49"></a>
### Q49: How to see why an auto-config was applied or not?
**Answer:** Enable the condition evaluation report (`--debug` or `debug=true`). It explains matched/unmatched `@Conditional` rules per auto-config class, which is invaluable for troubleshooting.

<a id="q50"></a>
### Q50: How to customize Jackson JSON settings?
**Answer:** Either declare a `Jackson2ObjectMapperBuilderCustomizer` bean or set `spring.jackson.*` properties. You can also define `ObjectMapper` beans explicitly when needed.

**Example:**
```java
@Bean Jackson2ObjectMapperBuilderCustomizer jsonCustomizer(){
  return b -> b.serializationInclusion(JsonInclude.Include.NON_NULL);
}
```

---

### Advanced/Ambiguous Level (Q51–Q100)

<a id="q51"></a>
### Q51: Diagnose circular dependency in Spring?
**Answer:** Cycles often appear with constructor injection between two services. Break the cycle by refactoring responsibilities, injecting an interface, or switching one side to lazy/setter injection. `ObjectProvider` and `@Lazy` can help, but refactoring is best.

**Example:** Using `ObjectProvider` to lazily resolve.
```java
@Service
class A { A(ObjectProvider<B> b) { this.b = b; } }
@Service
class B { B(A a) { /*...*/ } }
```

<a id="q52"></a>
### Q52: Why `@Transactional` sometimes doesn’t work?
**Answer:** Common causes: calling a `@Transactional` method from within the same class (self-invocation), annotating non-public methods, final methods preventing proxying, or missing a transaction manager. In reactive stacks, use reactive transactions instead.

**Example:** Move the transactional method to another bean or inject the proxied self.
```java
@Service class A { private final A self; A(A self){ this.self = self; } void doWork(){ self.txMethod(); } @Transactional public void txMethod(){ /*...*/ } }
```

<a id="q53"></a>
### Q53: How to handle N+1 queries in production?
**Answer:** Use fetch joins, `@EntityGraph`, or DTO projections to pre-fetch associations. Verify with SQL logging or tools like p6spy and assert query counts in tests.

**Example:**
```java
@EntityGraph(attributePaths = "items")
List<Order> findAll();
```

<a id="q54"></a>
### Q54: Difference between optimistic and pessimistic locking?
**Answer:** Optimistic locking adds a version column and detects conflicts at commit, ideal for low contention. Pessimistic locking acquires DB locks (`LockModeType.PESSIMISTIC_WRITE`) to prevent conflicts, suitable for high contention but can reduce concurrency.

**Example (optimistic):**
```java
@Version private Long version;
```

<a id="q55"></a>
### Q55: When to use `REQUIRES_NEW`?
**Answer:** For operations that must commit regardless of the outer transaction outcome (e.g., audit logs). It suspends the current transaction and starts a new one.

**Example:**
```java
@Transactional(propagation = Propagation.REQUIRES_NEW)
public void logAudit(Audit a){ repo.save(a); }
```

<a id="q56"></a>
### Q56: Idempotency for REST endpoints?
**Answer:** Ensure multiple identical requests have the same effect. PUT/DELETE should be idempotent by design. For POST that triggers side effects, use idempotency keys and de-duplication.

**Example:** Require `Idempotency-Key` header and store processed keys.

<a id="q57"></a>
### Q57: Implement request validation and error format?
**Answer:** Validate DTOs with Bean Validation, add message codes, and centralize error translation. Return machine-friendly error shapes with fields and codes for clients to act upon.

**Example:** See Q18 advice snippet.

<a id="q58"></a>
### Q58: Secure a stateless REST API?
**Answer:** Use bearer tokens (JWT/OAuth2), disable sessions, validate scopes/claims, and protect endpoints with method or URL security.

**Example (OAuth2 resource server):**
```java
http.oauth2ResourceServer(oauth2 -> oauth2.jwt());
```

<a id="q59"></a>
### Q59: `@PreAuthorize` with SpEL example?
**Answer:** Use SpEL to reference method args and authentication details.
```java
@PreAuthorize("hasRole('ADMIN') or #userId == authentication.name")
public UserDto find(String userId) { /*...*/ }
```

<a id="q60"></a>
### Q60: Propagate security context to `@Async` methods?
**Answer:** Wrap your executor with `DelegatingSecurityContextAsyncTaskExecutor` or use Spring’s default that propagates `SecurityContext` when configured.

**Example:**
```java
@Bean AsyncTaskExecutor taskExecutor() {
  return new DelegatingSecurityContextAsyncTaskExecutor(new SimpleAsyncTaskExecutor());
}
```

<a id="q61"></a>
### Q61: Avoid heavy controllers?
**Answer:** Thin controllers delegating to services; use DTO mappers; validation and error handling centralized.

<a id="q62"></a>
### Q62: How to tune HikariCP?
**Answer:** Align `maximumPoolSize` with DB and application concurrency, set sensible timeouts (`connectionTimeout`, `idleTimeout`, `maxLifetime`), and enable leak detection for development.

**Example:**
```
spring.datasource.hikari.maximum-pool-size=20
spring.datasource.hikari.connection-timeout=2500
spring.datasource.hikari.max-lifetime=1800000
```

<a id="q63"></a>
### Q63: Cache invalidation strategy?
**Answer:** Evict or update caches after writes (`@CacheEvict`/`@CachePut`), version keys to avoid collisions, and use TTLs to bound staleness.

**Example:**
```java
@CacheEvict(cacheNames="users", key="#user.id")
public void update(User user){ repo.save(user); }
```

<a id="q64"></a>
### Q64: Prevent cache stampede?
**Answer:** Use request coalescing (`sync=true` where supported), set staggered TTLs with jitter, and use distributed locks for hot keys.

<a id="q65"></a>
### Q65: Custom cache key generator?
**Answer:** Implement `KeyGenerator` to build composite keys when `key` SpEL gets unwieldy.
```java
@Bean KeyGenerator userKeyGen(){ return (t, m, p) -> "user:"+p[0]; }
```

<a id="q66"></a>
### Q66: Why not expose entities in APIs?
**Answer:** Leaks persistence concerns, risks lazy loading exceptions, and hinders versioning; use DTOs.

<a id="q67"></a>
### Q67: DTO mapping approaches?
**Answer:** Manual mapping for full control, `ModelMapper` for quick wins (runtime reflection), `MapStruct` for compile-time generation and performance.

**Example (MapStruct):**
```java
@Mapper interface UserMapper { UserDto toDto(User u); }
```

<a id="q68"></a>
### Q68: Backpressure in WebFlux?
**Answer:** Reactive streams support backpressure to avoid overwhelming consumers; use `Flux`/`Mono` operators.

<a id="q69"></a>
### Q69: When to choose MVC vs WebFlux?
**Answer:** MVC for blocking IO and simplicity; WebFlux for high-concurrency, non-blocking IO with reactive stacks.

<a id="q70"></a>
### Q70: How to add retries/circuit breakers?
**Answer:** Use Spring Retry for simple retries/backoff and Resilience4j for circuit breakers, bulkheads, and rate limiters. Configure policies per external call.

**Example (Resilience4j):**
```java
@CircuitBreaker(name = "catalog", fallbackMethod = "fallback")
public Product getProduct(String id){ /* remote call */ }
```

<a id="q71"></a>
### Q71: Observability setup?
**Answer:** Enable Actuator with metrics, add a Micrometer registry (Prometheus/OTel), propagate trace IDs in logs, and capture business KPIs with `@Timed` or custom meters.

<a id="q72"></a>
### Q72: Graceful shutdown in Boot?
**Answer:** Enable graceful shutdown and make sure executors and schedulers respect interrupts and timeouts.
```
server.shutdown=graceful
spring.lifecycle.timeout-per-shutdown-phase=30s
```

<a id="q73"></a>
### Q73: Database migrations best practice?
**Answer:** Use versioned migrations (Flyway/Liquibase), keep them immutable, review carefully, and run in CI/CD with rollback strategies.

<a id="q74"></a>
### Q74: Handling large file uploads?
**Answer:** Stream using `MultipartFile.transferTo` or reactive streaming; set size limits; scan content.

<a id="q75"></a>
### Q75: Avoid timezone bugs?
**Answer:** Store in UTC; use `Instant`/`OffsetDateTime`; set JVM TZ; convert at edges.

<a id="q76"></a>
### Q76: Testcontainers usage?
**Answer:** Start real services in tests for fidelity. Wire container URLs using `@DynamicPropertySource`.
```java
static PostgreSQLContainer<?> pg = new PostgreSQLContainer<>("postgres:16");
@DynamicPropertySource static void props(DynamicPropertyRegistry r){ r.add("spring.datasource.url", pg::getJdbcUrl); }
```

<a id="q77"></a>
### Q77: Slice tests vs full context?
**Answer:** Slice tests are faster and focused (`@WebMvcTest`, `@DataJpaTest`); full context for end-to-end integration.

<a id="q78"></a>
### Q78: `@DirtiesContext` downside?
**Answer:** Forces context reload, slowing test suite; use sparingly.

<a id="q79"></a>
### Q79: Custom Actuator endpoint?
**Answer:** Create an endpoint bean and opt-in to exposure if needed.
```java
@Endpoint(id = "greet")
class GreetEndpoint { @ReadOperation String hello(){ return "hi"; } }
```

<a id="q80"></a>
### Q80: Health indicator customization?
**Answer:** Implement `HealthIndicator` for dependency checks and include meaningful details guarded by security.
```java
@Component class DbHealth implements HealthIndicator { public Health health(){ return Health.up().withDetail("db","ok").build(); }}
```

<a id="q81"></a>
### Q81: Strategies for configuration secrets?
**Answer:** Use dedicated secret stores (Vault/AWS Secrets Manager), mount as env vars or files, and rotate regularly. Never commit secrets to VCS.

<a id="q82"></a>
### Q82: How to reduce startup time?
**Answer:** Remove unused starters, enable lazy init for non-critical beans, cache classpath metadata, and avoid heavy work in static initializers.
```
spring.main.lazy-initialization=true
```

<a id="q83"></a>
### Q83: Conditional beans by environment?
**Answer:** Use `@Profile("dev")` for environment-specific beans, `@ConditionalOnProperty` for feature flags, or create custom `Condition`s for complex checks.

<a id="q84"></a>
### Q84: `@EnableScheduling` example?
**Answer:**
```java
@EnableScheduling
@Configuration class Sched {}
@Scheduled(fixedDelay = 5_000) void poll(){ /* ... */ }
```

<a id="q85"></a>
### Q85: `@Async` pitfalls?
**Answer:** Self-invocation (no proxy), missing executor configuration, blocking operations inside async methods, and forgetting exception handling for `CompletableFuture`.

<a id="q86"></a>
### Q86: JPA flush modes?
**Answer:** `AUTO` flushes pending changes before certain queries; `COMMIT` delays flush until commit. Use `@Transactional(readOnly = true)` to reduce unnecessary flushes for read paths.

<a id="q87"></a>
### Q87: Event-driven architecture in Spring?
**Answer:** Use `ApplicationEventPublisher` and `@EventListener`; for inter-service, use Kafka/Rabbit with Spring Cloud Stream.

<a id="q88"></a>
### Q88: Outbox pattern?
**Answer:** Write domain changes and an "outbox" event row in the same DB transaction; a background publisher reliably reads and publishes those events to Kafka/Rabbit, ensuring no lost messages.

<a id="q89"></a>
### Q89: Handling large result sets?
**Answer:** Use pagination (`Pageable`) for UI and `Stream<T>`/cursor-based fetching for batch processing to avoid loading all rows into memory.
```java
@QueryHints(@QueryHint(name = HINT_FETCH_SIZE, value = "1000"))
Stream<User> streamAll();
```

<a id="q90"></a>
### Q90: Prevent duplicated scheduled executions?
**Answer:** In clustered deployments, coordinate with leader election or distributed locks (e.g., ShedLock, Redis, DB-based locks) to ensure single execution.

<a id="q91"></a>
### Q91: Why `final` classes/methods break AOP?
**Answer:** JDK proxies proxy interfaces only; CGLIB creates subclasses and can’t override `final` methods/classes. Avoid `final` on beans that need proxy-based features.

<a id="q92"></a>
### Q92: Migrate from XML to Java config?
**Answer:** Move XML bean definitions to `@Configuration` classes with `@Bean` methods; replace `<context:component-scan>` with `@ComponentScan`; keep property placeholders via Boot’s externalized config.

<a id="q93"></a>
### Q93: How to handle partial updates (PATCH)?
**Answer:** Accept JSON Merge Patch or JSON Patch, apply changes to the existing resource, validate, and persist. Ensure you handle `null` semantics correctly.

<a id="q94"></a>
### Q94: Difference between `map` and `flatMap` in reactive?
**Answer:** `map` applies a synchronous transform to each element; `flatMap` maps to a `Publisher` and merges results, allowing async composition. `concatMap` preserves ordering.

<a id="q95"></a>
### Q95: Apply input rate limiting?
**Answer:** Enforce per-client quotas using API gateways, token-bucket libraries (bucket4j), or Redis-based counters. Return `429 Too Many Requests` with `Retry-After`.

<a id="q96"></a>
### Q96: JSON serialization pitfalls?
**Answer:** Bi-directional JPA relations can cause infinite recursion; either use DTOs, Jackson refs (`@JsonManagedReference/@JsonBackReference`), or `@JsonIgnore`. Also be careful exposing lazy proxies.

<a id="q97"></a>
### Q97: Global exception structure suggestion?
**Answer:** Standardize an error format (timestamp, status, code, message, path, traceId) and document error codes. Include user-safe messages and internal trace IDs.

<a id="q98"></a>
### Q98: How to profile slow endpoints?
**Answer:** Use Actuator metrics, enable `http.server.requests`, add `@Timed` on critical methods, use APM/tracing (OpenTelemetry), and apply around advice for timing.

<a id="q99"></a>
### Q99: Containerization best practices?
**Answer:** Use slim JRE base, layered jars, proper JVM memory flags, health checks, and non-root user.

<a id="q100"></a>
### Q100: Where to find official docs?
**Answer:** Spring Framework Reference, Spring Boot Reference, Spring Data JPA, and Spring Security docs:
- `https://docs.spring.io/spring-framework/reference/`
- `https://docs.spring.io/spring-boot/docs/current/reference/html/`
- `https://docs.spring.io/spring-data/jpa/docs/current/reference/html/`
- `https://docs.spring.io/spring-security/reference/`


