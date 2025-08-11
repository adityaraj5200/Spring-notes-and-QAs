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
**Answer:** DI is a technique where the container provides object dependencies, decoupling construction from usage and enabling testability and modularity.

<a id="q2"></a>
### Q2: What is Inversion of Control (IoC)?
**Answer:** IoC is a principle where the framework controls object creation and wiring, not the application code. Spring implements IoC via the `ApplicationContext`.

<a id="q3"></a>
### Q3: Bean vs Component in Spring?
**Answer:** `@Bean` declares a bean from a method within `@Configuration`; `@Component` (and stereotypes like `@Service`) mark classes for component scanning.

<a id="q4"></a>
### Q4: Constructor vs Setter injection?
**Answer:** Prefer constructor for required, immutable dependencies; use setter for optional or late-bound dependencies.

<a id="q5"></a>
### Q5: What are common bean scopes?
**Answer:** `singleton` (default), `prototype`, and web scopes: `request`, `session`, `application`.

<a id="q6"></a>
### Q6: Describe the bean lifecycle.
**Answer:** Instantiate → populate properties → `@PostConstruct`/`afterPropertiesSet` → ready → `@PreDestroy`/`destroy`.

<a id="q7"></a>
### Q7: What is `ApplicationContext` vs `BeanFactory`?
**Answer:** Both are IoC containers; `ApplicationContext` is a superset with internationalization, events, AOP, and resource loading; used in most apps.

<a id="q8"></a>
### Q8: What does `@Primary` do?
**Answer:** Marks a bean as the default candidate when multiple beans of the same type exist and no `@Qualifier` is specified.

<a id="q9"></a>
### Q9: When do you need `@Qualifier`?
**Answer:** When multiple beans of the same type exist and you need to disambiguate which one to inject.

<a id="q10"></a>
### Q10: `@Component` vs `@Service` vs `@Repository`?
**Answer:** All are components; `@Service` signals business logic; `@Repository` adds exception translation for persistence.

<a id="q11"></a>
### Q11: What is auto-configuration in Spring Boot?
**Answer:** Conditional configuration that automatically sets up beans based on the classpath, environment, and other beans.

<a id="q12"></a>
### Q12: What are Spring Boot starters?
**Answer:** Curated dependency descriptors like `spring-boot-starter-web` that bundle common libraries for a feature.

<a id="q13"></a>
### Q13: Purpose of `@SpringBootApplication`?
**Answer:** Combines `@Configuration`, `@EnableAutoConfiguration`, and `@ComponentScan` with sensible defaults.

<a id="q14"></a>
### Q14: How to change server port in Boot?
**Answer:** Set `server.port=8081` in properties or `--server.port=8081` via CLI.

<a id="q15"></a>
### Q15: What is `@ConfigurationProperties`?
**Answer:** Binds externalized configuration to typed objects, supporting validation and metadata.

<a id="q16"></a>
### Q16: `@Value` vs `@ConfigurationProperties`?
**Answer:** `@Value` for single values; `@ConfigurationProperties` for grouped, hierarchical config with type safety.

<a id="q17"></a>
### Q17: What is `@RestController`?
**Answer:** A stereotype combining `@Controller` and `@ResponseBody` to return JSON/XML directly.

<a id="q18"></a>
### Q18: How to handle validation errors in REST?
**Answer:** Use `@Valid` on request DTOs and a `@ControllerAdvice` to handle `MethodArgumentNotValidException`.

<a id="q19"></a>
### Q19: Difference between `@RequestParam` and `@PathVariable`?
**Answer:** `@PathVariable` binds URI template variables; `@RequestParam` binds query parameters/form data.

<a id="q20"></a>
### Q20: How do `HttpMessageConverter`s work?
**Answer:** They serialize/deserialize HTTP bodies to/from objects based on content type and available converters (e.g., Jackson for JSON).

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
**Answer:** Proxy-based aspect framework to apply cross-cutting concerns like logging, security, and transactions.

<a id="q32"></a>
### Q32: Types of AOP advice?
**Answer:** `@Before`, `@AfterReturning`, `@AfterThrowing`, `@After`, `@Around`.

<a id="q33"></a>
### Q33: Common AOP pitfall?
**Answer:** Self-invocation bypasses proxies, so advice doesn’t apply to internal method calls.

<a id="q34"></a>
### Q34: Spring Security filter chain?
**Answer:** Ordered filters process requests for authentication/authorization; configured via `SecurityFilterChain`.

<a id="q35"></a>
### Q35: Password storage best practice?
**Answer:** Use strong `PasswordEncoder` like `BCryptPasswordEncoder`; never store plain text.

<a id="q36"></a>
### Q36: Enable method-level security?
**Answer:** Add `@EnableMethodSecurity` and annotate methods with `@PreAuthorize`, `@PostAuthorize`.

<a id="q37"></a>
### Q37: CSRF in REST APIs?
**Answer:** For stateless token-based APIs, CSRF can usually be disabled; for session-based apps, keep it enabled.

<a id="q38"></a>
### Q38: CORS configuration?
**Answer:** Use `CorsConfigurationSource` or `http.cors()` with allowed origins, methods, and headers.

<a id="q39"></a>
### Q39: How to handle exceptions globally?
**Answer:** `@ControllerAdvice` with typed `@ExceptionHandler` methods returning consistent error responses.

<a id="q40"></a>
### Q40: Difference between `@Controller` and `@RestController`?
**Answer:** `@Controller` returns views; `@RestController` returns bodies directly (JSON by default).

<a id="q41"></a>
### Q41: How to return 201 Created with Location header?
**Answer:** `ResponseEntity.created(URI).body(resource)`.

<a id="q42"></a>
### Q42: Purpose of Actuator?
**Answer:** Exposes operational endpoints (health, metrics, env, info) for monitoring and management.

<a id="q43"></a>
### Q43: Enable metrics export?
**Answer:** Add Micrometer registry (e.g., Prometheus) dependency; configure scrape path via Actuator.

<a id="q44"></a>
### Q44: How does property precedence work in Boot?
**Answer:** Command-line args > env vars > application-{profile}.properties > application.properties > defaults.

<a id="q45"></a>
### Q45: Profiles usage?
**Answer:** Activate with `spring.profiles.active` and condition beans with `@Profile("dev")` etc.

<a id="q46"></a>
### Q46: What does `@EnableAutoConfiguration` do?
**Answer:** Triggers Boot’s conditional configuration based on classpath and environment.

<a id="q47"></a>
### Q47: Example of custom `@ConfigurationProperties`?
**Answer:**
```java
@ConfigurationProperties(prefix="app.mail")
record MailProps(String host, int port) {}
```

<a id="q48"></a>
### Q48: How to disable a specific auto-configuration?
**Answer:** `@SpringBootApplication(exclude = DataSourceAutoConfiguration.class)` or property `spring.autoconfigure.exclude`.

<a id="q49"></a>
### Q49: How to see why an auto-config was applied or not?
**Answer:** Use `--debug` or set `debug=true` to view the Condition Evaluation Report in logs.

<a id="q50"></a>
### Q50: How to customize Jackson JSON settings?
**Answer:** Define a `Jackson2ObjectMapperBuilderCustomizer` bean or configure properties like `spring.jackson.serialization.*`.

---

### Advanced/Ambiguous Level (Q51–Q100)

<a id="q51"></a>
### Q51: Diagnose circular dependency in Spring?
**Answer:** Prefer constructor injection; if unavoidable, use setter injection or `ObjectProvider`; refactor to break cycles.

<a id="q52"></a>
### Q52: Why `@Transactional` sometimes doesn’t work?
**Answer:** Method not public, self-invocation, wrong proxy mode, or using reactive drivers without reactive transactions.

<a id="q53"></a>
### Q53: How to handle N+1 queries in production?
**Answer:** Add fetch joins, `@EntityGraph`, or projections; validate with SQL logs or profiling; write tests to assert query count.

<a id="q54"></a>
### Q54: Difference between optimistic and pessimistic locking?
**Answer:** Optimistic uses version checks to detect conflicts; pessimistic uses database locks to prevent them.

<a id="q55"></a>
### Q55: When to use `REQUIRES_NEW`?
**Answer:** For audit/logging or non-critical tasks that must succeed/fail independently of the outer transaction.

<a id="q56"></a>
### Q56: Idempotency for REST endpoints?
**Answer:** Make PUT/DELETE idempotent; use idempotency keys for POST on external calls; ensure safe retries.

<a id="q57"></a>
### Q57: Implement request validation and error format?
**Answer:** DTOs with `@Valid`; centralized `@ControllerAdvice` returning structured errors with codes.

<a id="q58"></a>
### Q58: Secure a stateless REST API?
**Answer:** Use JWTs or opaque tokens; disable sessions; enable CORS appropriately; add resource server config for OAuth2.

<a id="q59"></a>
### Q59: `@PreAuthorize` with SpEL example?
**Answer:** `@PreAuthorize("hasRole('ADMIN') or #id == authentication.name")`.

<a id="q60"></a>
### Q60: Propagate security context to `@Async` methods?
**Answer:** Use `DelegatingSecurityContextAsyncTaskExecutor` or enable `@EnableAsync` with proper strategy.

<a id="q61"></a>
### Q61: Avoid heavy controllers?
**Answer:** Thin controllers delegating to services; use DTO mappers; validation and error handling centralized.

<a id="q62"></a>
### Q62: How to tune HikariCP?
**Answer:** Configure pool size per CPU/DB limits, timeouts (`connectionTimeout`, `idleTimeout`), and leak detection.

<a id="q63"></a>
### Q63: Cache invalidation strategy?
**Answer:** Evict on writes with `@CacheEvict`; use version in key; consider TTL to avoid stale data.

<a id="q64"></a>
### Q64: Prevent cache stampede?
**Answer:** Add TTL with jitter; use `@Cacheable(sync=true)` (Caffeine), or distributed locks for hot keys.

<a id="q65"></a>
### Q65: Custom cache key generator?
**Answer:** Implement `KeyGenerator` and reference it in `@Cacheable(keyGenerator="beanName")`.

<a id="q66"></a>
### Q66: Why not expose entities in APIs?
**Answer:** Leaks persistence concerns, risks lazy loading exceptions, and hinders versioning; use DTOs.

<a id="q67"></a>
### Q67: DTO mapping approaches?
**Answer:** Manual mapping, `ModelMapper` (runtime), or `MapStruct` (compile-time, fast).

<a id="q68"></a>
### Q68: Backpressure in WebFlux?
**Answer:** Reactive streams support backpressure to avoid overwhelming consumers; use `Flux`/`Mono` operators.

<a id="q69"></a>
### Q69: When to choose MVC vs WebFlux?
**Answer:** MVC for blocking IO and simplicity; WebFlux for high-concurrency, non-blocking IO with reactive stacks.

<a id="q70"></a>
### Q70: How to add retries/circuit breakers?
**Answer:** Use Spring Retry or Resilience4j via Spring Cloud Circuit Breaker with policies per call.

<a id="q71"></a>
### Q71: Observability setup?
**Answer:** Enable Actuator metrics/traces, Micrometer registry (Prometheus/OTel), and structured JSON logging.

<a id="q72"></a>
### Q72: Graceful shutdown in Boot?
**Answer:** Enable `server.shutdown=graceful` and configure termination timeouts; ensure thread pools stop.

<a id="q73"></a>
### Q73: Database migrations best practice?
**Answer:** Use Flyway/Liquibase; versioned, repeatable scripts; run on startup in CI/CD; never auto-generate in prod.

<a id="q74"></a>
### Q74: Handling large file uploads?
**Answer:** Stream using `MultipartFile.transferTo` or reactive streaming; set size limits; scan content.

<a id="q75"></a>
### Q75: Avoid timezone bugs?
**Answer:** Store in UTC; use `Instant`/`OffsetDateTime`; set JVM TZ; convert at edges.

<a id="q76"></a>
### Q76: Testcontainers usage?
**Answer:** Spin up real DBs/queues in tests; integrate with `@DynamicPropertySource` for URLs.

<a id="q77"></a>
### Q77: Slice tests vs full context?
**Answer:** Slice tests are faster and focused (`@WebMvcTest`, `@DataJpaTest`); full context for end-to-end integration.

<a id="q78"></a>
### Q78: `@DirtiesContext` downside?
**Answer:** Forces context reload, slowing test suite; use sparingly.

<a id="q79"></a>
### Q79: Custom Actuator endpoint?
**Answer:** Implement `@Endpoint` or `@RestControllerEndpoint` and expose via management config.

<a id="q80"></a>
### Q80: Health indicator customization?
**Answer:** Implement `HealthIndicator` to report dependencies’ status; include details under proper security.

<a id="q81"></a>
### Q81: Strategies for configuration secrets?
**Answer:** Use environment variables, Vault/Secret Manager, or KMS; never commit secrets.

<a id="q82"></a>
### Q82: How to reduce startup time?
**Answer:** Lazy initialization, remove unused starters, optimize scanning, and avoid heavy static initialization.

<a id="q83"></a>
### Q83: Conditional beans by environment?
**Answer:** Use `@Profile`, `@ConditionalOnProperty`, or custom `@Condition`.

<a id="q84"></a>
### Q84: `@EnableScheduling` example?
**Answer:**
```java
@Scheduled(fixedDelay = 5_000)
void poll(){ /* ... */ }
```

<a id="q85"></a>
### Q85: `@Async` pitfalls?
**Answer:** Self-invocation, missing executor, and blocking calls negate benefits.

<a id="q86"></a>
### Q86: JPA flush modes?
**Answer:** `AUTO` flushes before queries; `COMMIT` flushes at transaction commit.

<a id="q87"></a>
### Q87: Event-driven architecture in Spring?
**Answer:** Use `ApplicationEventPublisher` and `@EventListener`; for inter-service, use Kafka/Rabbit with Spring Cloud Stream.

<a id="q88"></a>
### Q88: Outbox pattern?
**Answer:** Persist events in the same transaction as DB changes and publish asynchronously to ensure reliability.

<a id="q89"></a>
### Q89: Handling large result sets?
**Answer:** Use paging/streaming (`Stream<T>` in Spring Data) and server-side cursors.

<a id="q90"></a>
### Q90: Prevent duplicated scheduled executions?
**Answer:** Use distributed locks or leader election; ensure single instance executes task.

<a id="q91"></a>
### Q91: Why `final` classes/methods break AOP?
**Answer:** CGLIB proxying can’t override final methods; JDK proxies only proxy interfaces.

<a id="q92"></a>
### Q92: Migrate from XML to Java config?
**Answer:** Replace XML with `@Configuration` classes and `@Bean` methods; leverage component scanning.

<a id="q93"></a>
### Q93: How to handle partial updates (PATCH)?
**Answer:** Use `@PatchMapping` with JSON Merge Patch/JSON Patch and validate updated fields.

<a id="q94"></a>
### Q94: Difference between `map` and `flatMap` in reactive?
**Answer:** `map` transforms value; `flatMap` transforms to a publisher and flattens asynchronously.

<a id="q95"></a>
### Q95: Apply input rate limiting?
**Answer:** Use gateway filters, bucket4j, or Redis-based rate limiters per client.

<a id="q96"></a>
### Q96: JSON serialization pitfalls?
**Answer:** Infinite recursion in bi-directional relations; use DTOs, `@JsonManagedReference`/`@JsonBackReference`, or `@JsonIgnore`.

<a id="q97"></a>
### Q97: Global exception structure suggestion?
**Answer:** `{ timestamp, status, error, message, path, traceId }` with clear, documented error codes.

<a id="q98"></a>
### Q98: How to profile slow endpoints?
**Answer:** Use Actuator metrics, `@Timed`, logs, and APM tools; add around advice for timing.

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


