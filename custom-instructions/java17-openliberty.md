# Copilot Custom Instructions: Java 17 Open Liberty Web Application

## Project Context
You are working on a Java 17 web application deployed on IBM Open Liberty server using Gradle as the build tool. The application does NOT use Spring Framework or Spring Boot. Focus on Jakarta EE specifications, Open Liberty features, and enterprise Java best practices.

## Technology Stack
- **Java Version**: Java 17 (LTS)
- **Application Server**: IBM Open Liberty
- **Build Tool**: Gradle
- **Enterprise Framework**: Jakarta EE (NO Spring/Spring Boot)
- **Specifications**: JAX-RS, CDI, JSON-P, JSON-B, Bean Validation (NO Database/JPA)

## Development Guidelines

### Java 17 Standards
- **Modern Java Features**: Utilize Java 17 features including records, pattern matching, sealed classes, text blocks
- **Module System**: Use Java modules (module-info.java) when appropriate
- **Memory Management**: Leverage improved garbage collection and memory efficiency
- **Security**: Implement modern security practices with updated cryptographic standards

### Open Liberty Architecture
- **Server Configuration**: Use `server.xml` for feature configuration and datasource setup
- **Feature Management**: Enable only required Liberty features for optimal performance
- **Microservices**: Design with microservices architecture using Liberty's lightweight runtime
- **Cloud Native**: Follow cloud-native patterns with Liberty's container optimization

### Jakarta EE Implementation
- **Dependency Injection**: Use CDI (Contexts and Dependency Injection) for dependency management
- **RESTful Services**: Implement JAX-RS for REST API development
- **Data Processing**: Use in-memory data structures, external APIs, or file-based storage
- **JSON Processing**: Utilize JSON-P and JSON-B for JSON handling
- **Validation**: Apply Bean Validation for input validation
- **Configuration**: Use MicroProfile Config for externalized configuration

## Code Structure Standards

### Project Structure
```
src/
├── main/
│   ├── java/
│   │   └── com/company/app/
│   │       ├── config/           # Configuration classes
│   │       ├── model/            # Domain models and POJOs
│   │       ├── repository/       # Data access layer (external APIs, file I/O)
│   │       ├── service/          # Business logic
│   │       ├── rest/             # JAX-RS resources
│   │       ├── dto/              # Data Transfer Objects
│   │       ├── exception/        # Custom exceptions
│   │       ├── client/           # External API clients
│   │       └── util/             # Utility classes
│   ├── resources/
│   │   ├── META-INF/
│   │   │   └── microprofile-config.properties  # MicroProfile configuration
│   │   └── WEB-INF/
│   │       └── beans.xml         # CDI configuration
│   └── liberty/
│       └── config/
│           └── server.xml        # Liberty server configuration
├── test/
│   ├── java/                     # Unit and integration tests
│   └── resources/
└── build.gradle                  # Gradle build configuration
```

### Jakarta EE Component Examples

#### JAX-RS REST Resource
```java
@Path("/api/users")
@ApplicationScoped
@Produces(MediaType.APPLICATION_JSON)
@Consumes(MediaType.APPLICATION_JSON)
public class UserResource {
    
    @Inject
    private UserService userService;
    
    @GET
    @Path("/{id}")
    public Response getUserById(@PathParam("id") Long id) {
        try {
            User user = userService.findById(id);
            return Response.ok(user).build();
        } catch (EntityNotFoundException e) {
            return Response.status(Response.Status.NOT_FOUND).build();
        }
    }
    
    @POST
    public Response createUser(@Valid UserDTO userDTO) {
        User user = userService.create(userDTO);
        return Response.status(Response.Status.CREATED)
                      .entity(user)
                      .build();
    }
}
```

#### CDI Service Layer
```java
@ApplicationScoped
public class UserService {
    
    @Inject
    private UserRepository userRepository;
    
    @Inject
    private Event<UserCreatedEvent> userCreatedEvent;
    
    @Inject
    @ConfigProperty(name = "app.user.max-count", defaultValue = "1000")
    private int maxUserCount;
    
    public User create(UserDTO userDTO) {
        // Validate business rules
        validateUser(userDTO);
        
        User user = new User();
        user.setId(UUID.randomUUID().toString());
        user.setName(userDTO.getName());
        user.setEmail(userDTO.getEmail());
        user.setCreatedAt(LocalDateTime.now());
        
        user = userRepository.save(user);
        userCreatedEvent.fire(new UserCreatedEvent(user.getId()));
        
        return user;
    }
    
    public Optional<User> findById(String id) {
        return userRepository.findById(id);
    }
    
    public List<User> findAll() {
        return userRepository.findAll();
    }
    
    private void validateUser(UserDTO userDTO) {
        if (userRepository.countAll() >= maxUserCount) {
            throw new BusinessException("Maximum user count exceeded");
        }
        
        if (userRepository.existsByEmail(userDTO.getEmail())) {
            throw new BusinessException("Email already exists");
        }
    }
}
```

#### Data Repository (Non-JPA)
```java
@ApplicationScoped
public class UserRepository {
    
    private final Map<String, User> userStorage = new ConcurrentHashMap<>();
    
    @Inject
    private ExternalApiClient externalApiClient;
    
    public User save(User user) {
        userStorage.put(user.getId(), user);
        return user;
    }
    
    public Optional<User> findById(String id) {
        return Optional.ofNullable(userStorage.get(id));
    }
    
    public List<User> findAll() {
        return new ArrayList<>(userStorage.values());
    }
    
    public boolean existsByEmail(String email) {
        return userStorage.values().stream()
                         .anyMatch(user -> user.getEmail().equals(email));
    }
    
    public int countAll() {
        return userStorage.size();
    }
    
    public void delete(String id) {
        userStorage.remove(id);
    }
    
    // Method to sync with external API
    public void syncWithExternalSource() {
        List<User> externalUsers = externalApiClient.fetchUsers();
        externalUsers.forEach(user -> userStorage.put(user.getId(), user));
    }
}
```

#### Domain Model (POJO)
```java
public class User {
    
    private String id;
    
    @NotBlank(message = "Name is required")
    @Size(max = 100, message = "Name must not exceed 100 characters")
    private String name;
    
    @Email(message = "Invalid email format")
    @NotBlank(message = "Email is required")
    private String email;
    
    private LocalDateTime createdAt;
    
    private LocalDateTime updatedAt;
    
    // Constructors
    public User() {}
    
    public User(String name, String email) {
        this.id = UUID.randomUUID().toString();
        this.name = name;
        this.email = email;
        this.createdAt = LocalDateTime.now();
        this.updatedAt = LocalDateTime.now();
    }
    
    // Getters and setters
    public String getId() { return id; }
    public void setId(String id) { this.id = id; }
    
    public String getName() { return name; }
    public void setName(String name) { 
        this.name = name; 
        this.updatedAt = LocalDateTime.now();
    }
    
    public String getEmail() { return email; }
    public void setEmail(String email) { 
        this.email = email; 
        this.updatedAt = LocalDateTime.now();
    }
    
    public LocalDateTime getCreatedAt() { return createdAt; }
    public void setCreatedAt(LocalDateTime createdAt) { this.createdAt = createdAt; }
    
    public LocalDateTime getUpdatedAt() { return updatedAt; }
    public void setUpdatedAt(LocalDateTime updatedAt) { this.updatedAt = updatedAt; }
    
    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        User user = (User) o;
        return Objects.equals(id, user.id);
    }
    
    @Override
    public int hashCode() {
        return Objects.hash(id);
    }
    
    @Override
    public String toString() {
        return "User{" +
                "id='" + id + '\'' +
                ", name='" + name + '\'' +
                ", email='" + email + '\'' +
                ", createdAt=" + createdAt +
                '}';
    }
}
```

## Gradle Build Configuration

### build.gradle Structure
```gradle
plugins {
    id 'java'
    id 'war'
    id 'io.openliberty.tools.gradle.Liberty' version '3.8.2'
}

group = 'com.company'
version = '1.0.0'

java {
    toolchain {
        languageVersion = JavaLanguageVersion.of(17)
    }
}

repositories {
    mavenCentral()
}

dependencies {
    // Jakarta EE API
    providedCompile 'jakarta.platform:jakarta.jakartaee-api:10.0.0'
    
    // Open Liberty Features
    libertyRuntime 'io.openliberty:openliberty-runtime:23.0.0.12'
    
    // HTTP Client for external APIs
    implementation 'org.apache.httpcomponents.client5:httpclient5:5.3'
    
    // JSON Processing
    implementation 'org.eclipse:yasson:3.0.3'
    
    // Validation
    implementation 'org.hibernate.validator:hibernate-validator:8.0.1.Final'
    
    // Configuration
    implementation 'org.eclipse.microprofile.config:microprofile-config-api:3.0.3'
    
    // Testing
    testImplementation 'org.junit.jupiter:junit-jupiter:5.10.1'
    testImplementation 'org.mockito:mockito-core:5.7.0'
    testImplementation 'org.testcontainers:junit-jupiter:1.19.3'
    testImplementation 'com.github.tomakehurst:wiremock-jre8:2.35.0'
    
    // Logging
    implementation 'org.slf4j:slf4j-api:2.0.9'
    implementation 'ch.qos.logback:logback-classic:1.4.14'
    
    // Utilities
    implementation 'org.apache.commons:commons-lang3:3.14.0'
    implementation 'com.google.guava:guava:32.1.3-jre'
}

liberty {
    server {
        name = 'defaultServer'
        configDirectory = file("src/main/liberty/config")
        serverXmlFile = file("src/main/liberty/config/server.xml")
        bootstrapProperties = ['default.http.port': '9080', 'default.https.port': '9443']
    }
}

test {
    useJUnitPlatform()
    testLogging {
        events "passed", "skipped", "failed"
    }
}
```

## Open Liberty Configuration

### server.xml Configuration
```xml
<server description="Java 17 Web Application Server">
    <featureManager>
        <feature>jakartaee-10.0</feature>
        <feature>microProfile-6.1</feature>
        <feature>localConnector-1.0</feature>
    </featureManager>
    
    <variable name="default.http.port" defaultValue="9080"/>
    <variable name="default.https.port" defaultValue="9443"/>
    
    <basicRegistry id="basic" realm="BasicRealm">
        <user name="admin" password="admin123" />
    </basicRegistry>
    
    <httpEndpoint id="defaultHttpEndpoint"
                  httpPort="${default.http.port}"
                  httpsPort="${default.https.port}" />
    
    <!-- REST Client Configuration -->
    <restConnector id="defaultRestConnector" />
    
    <!-- Application Configuration -->
    <webApplication location="app.war" contextRoot="/">
        <classloader apiTypeVisibility="spec,ibm-api,third-party"/>
    </webApplication>
    
    <!-- MicroProfile Config -->
    <mpConfig>
        <configSource>
            <appProperties>
                <property name="app.name" value="Java17WebApp"/>
                <property name="app.version" value="1.0.0"/>
                <property name="app.user.max-count" value="1000"/>
                <property name="external.api.base-url" value="https://api.example.com"/>
                <property name="external.api.timeout" value="30000"/>
            </appProperties>
        </configSource>
    </mpConfig>
    
    <!-- CORS Configuration -->
    <cors domain="/api"
          allowedOrigins="http://localhost:3000, https://yourdomain.com"
          allowedMethods="GET, POST, PUT, DELETE, OPTIONS"
          allowedHeaders="Content-Type, Authorization"
          maxAge="3600" />
    
    <logging traceSpecification="*=info:com.company.app.*=fine" maxFileSize="20" maxFiles="10"/>
</server>
```

## Best Practices and Strategies

### Java 17 Optimization
- **Records**: Use records for immutable data classes and DTOs
- **Pattern Matching**: Leverage pattern matching for cleaner conditional logic
- **Text Blocks**: Use text blocks for SQL queries and JSON templates
- **Sealed Classes**: Implement sealed classes for controlled inheritance hierarchies
- **Switch Expressions**: Use modern switch expressions for cleaner code

### Open Liberty Performance
- **Feature Optimization**: Enable only required Liberty features to minimize memory footprint
- **Memory Management**: Configure appropriate heap sizes and garbage collection settings
- **Caching**: Implement in-memory caching strategies for frequently accessed data
- **Health Checks**: Implement MicroProfile Health checks for monitoring
- **Metrics**: Use MicroProfile Metrics for application monitoring
- **Connection Management**: Optimize HTTP client connections for external APIs

### Jakarta EE Architecture
- **CDI Scopes**: Use appropriate CDI scopes (@ApplicationScoped, @RequestScoped, @SessionScoped)
- **Event Handling**: Implement CDI events for decoupled communication
- **Exception Handling**: Create custom exception mappers for JAX-RS
- **Validation**: Use Bean Validation with custom validators
- **Security**: Implement authentication and authorization using Jakarta Security
- **Configuration**: Use MicroProfile Config for externalized configuration

### Testing Strategy
- **Unit Tests**: Test business logic with JUnit 5 and Mockito
- **Integration Tests**: Test complete workflows with external API mocking
- **REST API Tests**: Test JAX-RS endpoints with REST Assured
- **Liberty Testing**: Use Liberty's built-in testing features
- **External API Testing**: Use WireMock for external API testing
- **Test Profiles**: Separate test configurations for different environments

### Security Implementation
- **Authentication**: Implement JWT-based authentication
- **Authorization**: Use role-based access control
- **Input Validation**: Validate all inputs using Bean Validation
- **SQL Injection Prevention**: Use parameterized queries
- **CORS**: Configure CORS policies appropriately

### Data Management
- **In-Memory Storage**: Use concurrent data structures for thread-safe operations
- **External APIs**: Implement proper HTTP client management and error handling
- **File Operations**: Handle file I/O operations with proper exception handling
- **Caching**: Implement intelligent caching strategies for external data
- **Data Validation**: Validate all data inputs and outputs thoroughly

## Code Generation Instructions

### When Creating Components:
1. **JAX-RS Resources**: Always include proper annotations, exception handling, and validation
2. **CDI Services**: Use appropriate scopes and inject dependencies properly
3. **JPA Entities**: Include proper annotations, validation, and audit fields
4. **DTOs**: Use Java records where appropriate for immutable data transfer
5. **Exception Handling**: Create custom exceptions with proper error responses

### When Writing Tests:
1. **Unit Tests**: Focus on business logic with proper mocking
2. **Integration Tests**: Test complete workflows with real dependencies
3. **REST Tests**: Verify API contracts and error handling
4. **Database Tests**: Use Testcontainers for realistic database testing
5. **Performance Tests**: Include load testing for critical endpoints

### When Configuring Liberty:
1. **Minimal Features**: Enable only required Liberty features
2. **Environment Variables**: Use environment variables for configuration
3. **Security**: Configure proper security settings
4. **Monitoring**: Enable health checks and metrics
5. **Logging**: Configure appropriate logging levels

## Development Workflow

### Code Quality Standards
- **Code Style**: Follow Google Java Style Guide
- **Documentation**: Include JavaDoc for public APIs
- **Error Handling**: Implement comprehensive error handling
- **Performance**: Consider performance implications in design
- **Security**: Apply security best practices throughout

### Build and Deployment
- **Gradle Tasks**: Use Liberty Gradle plugin for development
- **Environment Configuration**: Support multiple environments
- **Container Deployment**: Optimize for container deployment
- **Health Monitoring**: Include health check endpoints
- **Logging**: Implement structured logging

### Monitoring and Maintenance
- **Application Metrics**: Implement MicroProfile Metrics
- **Health Checks**: Use MicroProfile Health for readiness/liveness probes
- **Distributed Tracing**: Implement OpenTracing for microservices
- **Log Aggregation**: Use structured logging for log aggregation
- **Performance Monitoring**: Monitor JVM and application performance

## Response Format Guidelines

When providing code solutions, always include:
1. **Complete Implementation**: Provide fully functional code examples
2. **Configuration**: Include necessary server.xml and build.gradle configurations
3. **Testing**: Provide comprehensive test examples
4. **Documentation**: Include JavaDoc and inline comments
5. **Best Practices**: Explain architectural decisions and optimizations
6. **Error Handling**: Include proper exception handling and logging

## Restrictions and Preferences

### Mandatory Patterns
- **Jakarta EE**: Use Jakarta EE specifications, not legacy Java EE
- **CDI**: Use CDI for dependency injection, not manual instantiation
- **JAX-RS**: Use JAX-RS for REST APIs, not servlets
- **In-Memory/External APIs**: Use in-memory storage or external APIs, not databases
- **Java 17**: Utilize Java 17 features and improvements
- **MicroProfile Config**: Use MicroProfile Config for externalized configuration

### Forbidden Patterns
- **Spring Framework**: Do not suggest Spring or Spring Boot
- **Database/JPA**: Avoid suggesting database operations or JPA entities
- **Legacy Java EE**: Avoid javax.* packages, use jakarta.*
- **Manual DI**: Do not create manual dependency injection
- **Outdated Java**: Avoid pre-Java 17 patterns and features
- **Heavyweight Solutions**: Avoid complex frameworks when Liberty features suffice

This instruction set ensures all code generation and recommendations align with Java 17, Open Liberty, and Jakarta EE best practices while maintaining enterprise-grade quality and performance standards.