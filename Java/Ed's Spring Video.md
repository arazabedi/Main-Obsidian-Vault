What we'll be using:
Maven - NPM
Spring Data MongoDB - Mongoose
Spring Data Rest - Express (partial replacement)
Spring Web Services - Express (partial replacement)

Start here:
start.spring.io - Spring Initializr (Use Java 17 and Maven)

Add these dependencies:
**Spring Web**
**Rest Repositories**
**Spring Data MongoDB**

Application.properties is like the .env file:
```
// src/main/resources/application.properties

server.port=4000
spring.data.mongodb.host=127.0.0.1  
spring.data.mongodb.port=27017  
spring.data.mongodb.database=todos
```

Make a model directory. Create a class for each collection, and add the `@document([mongodb collection])` annotation:

```java
@document("todos")
public class Todo {
// POJO (Plain Old Java Object) consisting of the properties that are in the mongodb collection.Include getters and setters

@Id  
@JsonProperty("id")  
private String _id;  
  
@JsonProperty("todoDescription")  
@NotEmpty(message="Todo must have a description")  
private String todoDescription;  
  
@JsonProperty("todoDateCreated")  
@NotEmpty(message="Todo must have a date created")  
private String todoDateCreated;  
  
@JsonProperty("todoCompleted")  
private boolean todoCompleted;  
  
public String get_id() {  
    return _id;  
}  
  
public void set_id(String _id) {  
    this._id = _id;  
}  
  
public String getTodoDescription() {  
    return todoDescription;  
}  
  
public void setTodoDescription(String todoDescription) {  
    this.todoDescription = todoDescription;  
}  
  
public String getTodoDateCreated() {  
    return todoDateCreated;  
}  
  
public void setTodoDateCreated(String todoDateCreated) {  
    this.todoDateCreated = todoDateCreated;  
}  
  
public boolean isTodoCompleted() {  
    return todoCompleted;  
}  
  
public void setTodoCompleted(boolean todoCompleted) {  
    this.todoCompleted = todoCompleted;  
}
```

To connect with the mongodb database, make a repositories directory, and inside it a ToDoRepository interface.

The repository (not like a github repo, but a spring way of getting access to the database) should extend the MongoRepository class, and the generic types should be the model and then the type of the id for that model.

```java
import org.springframework.data.mongodb.repository.MongoRepository;

public interface TodoRepository extends MongoRepository <ToDo, String> {

}
```

Here's the TodoController:

```java
package com.digitalfutures.academy.spring_demo.controllers;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class TodoController {

    @GetMapping(value="/todos")
    public String getAllTodos() {
        return "Getting all todos";
    }

    @PostMapping(value="/todos")
    public String addTodo() {
        return "Adding a todo";
    }
}

```

And the TodoServices. Spring takes care of dependency injection (creating an instance of TodoRepository and using it in TodoServices).


For Auth:
```xml
<dependency>  
    <groupId>org.springframework.boot</groupId>  
    <artifactId>spring-boot-starter-security</artifactId>  
</dependency>
```

This will require username and password - without writing any code just by adding the dependency (username is user, password is generated in the console when starting the app).

In Postman go to Authorization tab, select Basic Auth for Auth Type, and input the username/password. You should then be able to access endpoints.

# Validations

I wanted to provide logic for validations such as the email property in the user model. However the Jakarta package already contains many such validations as annotations, greatly reducing workload.

# Password

The password was originally handled in the user class itself, but that isn't secure because the password might appear in logs related to the user class. The password also exists in memory as a String which is immutable in Java making it difficult to secure. There's also no clear separation between password transport and password storage.

# Try/Catch

Spring handles exceptions itself without needing explicit try/catch statements in your controller methods. You can define global exception handlers using @ControllerAdvice or @restControllerAdvice annotation that act as centralised exception handlers across the entire application.

**Default Exception Resolution**: When an uncaught exception occurs in a controller method, Spring's default exception handling mechanism kicks in.

Automatic handling: 
- `IllegalArgumentException` → 400 Bad Request
- `HttpMessageNotReadableException` → 400 Bad Request
- `MethodArgumentNotValidException` → 400 Bad Request
- `NoHandlerFoundException` → 404 Not Found
- Other exceptions → 500 Internal Server Error

**Basic Error Response**: Spring generates a simple error response with minimal details.

### **TL;DR**

🔹 **No try-catch is needed in `sendFriendRequest()`.** Spring **automatically rolls back** for unchecked exceptions.  
🔹 Use `try-catch` **only if** you want to log, wrap checked exceptions, or customize responses.

# MongoDB

The database contains collections and documents.

A user object represents a document in the users collection.

Collection name is assumed by Spring to be the singular and lower case version of the class name (User -> users).

### **Key Takeaways:**

- **Collection** = Equivalent to a table in SQL. In this case, `users`.
- **Document** = Equivalent to a row in SQL. Each `User` object is a document.
- **Field** = Equivalent to a column in SQL. (`username`, `email`, `_id`).


# @Autowired

Only necessary when doing field injection (you should just use final and a constructor/@AllArgsConstructor instread) or when there are multiple constructors.

# Spring dependency injection

When Spring starts, it looks for `@Component`, `@Service`, `@Repository`, or `@Controller`.

These become Spring-managed beans.

```java
@Service
public class UserService { ... }
```

`UserService` becomes a Spring bean and is eligible for dependency injection.

After discovering a class as a Spring bean, Spring instantiates it. If the class has dependencies (as in a field that requires an object of some other class), then Spring creates the object and passes it to the bean (the class which is annotated as a bean).

|Injection Type|When Injection Happens?|Notes|
|---|---|---|

|   |   |   |
|---|---|---|
|**Constructor Injection** (Recommended)|**During object creation**|Ensures all dependencies are provided at once. No need for `@Autowired` since Spring auto-injects single-constructor dependencies.|

|   |   |   |
|---|---|---|
|**Field Injection** (`@Autowired` on fields)|**After object creation**|Spring injects dependencies **after** the object is created, using reflection. Not recommended due to lack of testability.|

|   |   |   |
|---|---|---|
|**Setter Injection** (`@Autowired` on setters)|**After object creation**|Spring creates the object first, then calls setter methods to inject dependencies.|

If a bean has a method annotated with `@PostConstruct`, Spring calls it **after dependency injection**. This is useful for initializing values that require dependencies.

### **📝 When Exactly Does Spring Inject?**

🔹 **At runtime**, during the **Spring application context initialization**.  
🔹 **Before** the bean is used but **after** it is created.  
🔹 **Immediately after instantiation** (for constructor injection).  
🔹 **Just before use** (for field or setter injection).

Would you like an example to see this in action with logs? 🚀

# How Spring Basically Works

- Spring is started in the Main method:

```java
@SpringBootApplication
public class CalCountApplication {

	public static void main(String[] args) {
		SpringApplication.run(CalCountApplication.class, args);
	}
}
```

- Spring loads all beans (classes annotated with `@Component`, `@Service`, `@Repository`, or `@Controller`).
- Spring injects dependencies and manages lifecycles.
- This allows you to focus on logic instead of object creation.
# Why the User constructor doesn't have a password property

Passwords shouldn't be stored in plain text. Having the password in the constructor could lead to the password being stored as plain text and stored in the user object.

You are handling the password in a more controlled way, hashing it before storing it. The hashed version is irreversable.

The `User` constructor likely doesn't have a password property because:

- **Security best practices** dictate that passwords should never be handled in plain text, especially in user constructors.
- It promotes a **clean separation of concerns** between the `User` class (representing user data) and the `PasswordService` (responsible for securely handling passwords).
- It reduces the risk of accidental exposure of sensitive data.
- It allows greater **flexibility** in handling password-related changes.

# Why JWT auth over traditional session-based auth?

- Stateless - server doesn't store session data, each request carries the token and the server can verify without tracking state (in traditional session auth the server stores session data which is more resource intensive).
- Scalability - No need to sync session data between servers if scaling to multiple servers
- Cross-domain - the JWT can be shared across different domains/services instead of being tied to one domain.
- Performance - No session data/lookup required
- Security - No session management so no hijacking or session fixation
- Flexibility - Can embed permissions and roles into JWT payload
- Expiration control - reduces risk of compromised or stale sessions
# @Configuration and Beans

- A class with this annotation can contain methods which you annotate with @Bean
- A Spring Bean is a Java object created, managed, and configured by the Spring IoC (Inversion of Control) container.
- Beans are the building blocks of the application.

Journey of a bean:
1. Definition - through @Component (or @Service etc.), or @Bean methods in @Configuration
2. Instantiation - when the app starts, Spring reads these definitions and creates instances of your beans
3. Dependency injection - Spring examines each bean to see what other beans it needs (its dependencies) and "wires" them together.
4. Configuration - Spring applies any additional configuration - like setting properties.
5. Lifecycle callbacks - If the bean implements certain interfaces or has annotated methods like @PostConstruct, Spring calls these methods at the appropriate times.
6. Usage - Now your beans are ready to be used by your application code
7. Destruction - When the app shuts down , Spring can call cleanup methods (@PreDestroy) before the bean is removed from memory.

# Filters

Create a class that extends OncePerRequestFilter and make a doFilterInternal method that takes a request, response and filterChain:

```java
@Override
protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain filterChain) {
...
// Continue the filter chain  
filterChain.doFilter(request, response);
}
```

You can create multiple filter classes.

Then add the filter object as a parameter in the SecurityConfig file (class annotated @Configuration):

```java
public SecurityFilterChain securityFilterChain(HttpSecurity http, JwtAuthenticationFilter jwtAuthenticationFilter) throws Exception {
        http
                .csrf(csrf -> csrf.disable()) // Make sure CSRF is completely disabled
                .cors(cors -> cors.disable())
                .sessionManagement(session -> session.sessionCreationPolicy(SessionCreationPolicy.STATELESS))
                .authorizeHttpRequests(auth -> auth
                        .requestMatchers("/api/auth/**").permitAll()  // Allow all auth endpoints
                        .requestMatchers("/api/auth/login").permitAll()  // Explicitly permit login
                        .requestMatchers("/api/auth/register").permitAll()  // Explicitly permit register
                        .anyRequest().authenticated())  // All other requests need authentication
                .addFilterBefore(jwtAuthenticationFilter, UsernamePasswordAuthenticationFilter.class);

        return http.build();
    }
```

For example if you make a new filter class you would add newFilter to the parameters and the addFilterBefore method parameters.

# Ambiguous mapping issue (two maps to the same path)

```java
@GetMapping(value = "/api/users")  
public List<User> getAllUsers() {  
    return userService.getAllUsers();  
}  
  
@GetMapping(value = "/api/users")  
public User searchForUserByUsername(@RequestParam String username) {  
    return userService.findByUsername(username);  
}
```

Spring MVC doesn't differentiate between endpoints based on the presence or absence of query parameters. From Spring's perspective, both methods map to `GET /api/users`, and it doesn't know which one to route to when that endpoint is called.

Different options:

**Modify your search endpoint to use a more specific path:**

```java
@GetMapping(value = "/api/users/search")
public User searchForUserByUsername(@RequestParam String username) {
    return userService.findByUsername(username);
}
```

**Combine the functionality into a single method:**

```java
    @GetMapping(value = "/api/users")
    public Object getUsersOrSearch(@RequestParam(required = false) String username) {
        if (username != null) {
            return userService.findByUsername(username);
        } else {
            return userService.getAllUsers();
        }
    }
```

or **Use a path variable instead:**

```java
@GetMapping(value = "/api/users/{username}")
public User searchForUserByUsername(@PathVariable String username) {
    return userService.findByUsername(username);
}
```

The first option of using `/api/users/search` for your search endpoint is typically considered best practice for several reasons:

1. **Clear separation of concerns**: It explicitly indicates the intent of the endpoint. The base endpoint `/api/users` returns all users, while `/api/users/search` clearly communicates that it's a search operation.
2. **REST convention alignment**: In RESTful design, the base resource endpoint (`/api/users`) typically returns the collection of resources, while specialized operations are indicated by additional path segments or sub-resources.
3. **Scalability for search options**: If you later want to add more search parameters (like searching by email, name, role, etc.), having a dedicated search endpoint gives you the flexibility to accept multiple optional parameters without confusing the API semantics.
4. **API discoverability**: When developers explore your API, the distinct endpoint names make it immediately clear what each endpoint does.
5. **Documentation clarity**: In API documentation, having separate, well-named endpoints makes your API easier to understand and use correctly.

The path variable approach (`/api/users/{username}`) is also valid, but it typically implies fetching a specific user by primary identifier rather than performing a search operation. It's better suited for retrieving a specific user by their unique ID rather than a search where multiple results might be possible.

# Why don't we mock the User class in unit tests?

### **1. Unit Tests Should Verify Real Behavior**

A unit test should check how the method manipulates object state. If we mock `User`, we lose the ability to verify real field changes. Instead, we'd have to stub methods like:

java

CopyEdit

```java
when(user.getSentRequests()).thenReturn(new HashSet<>());
```

This makes the test more about setting up mocks than actually verifying logic.

### **2. Mocks Don't Retain State**

Mockito creates **dummy objects** that return predefined values, but they don’t store changes unless explicitly stubbed.  
For example, this would fail with a mock:

java

CopyEdit

```java
user.getSentRequests().add("user2"); assertTrue(user.getSentRequests().contains("user2"));  // Fails if user is mocked!
```

If `User` is a mock, `getSentRequests()` doesn't return the modified set unless we manually program it to, which defeats the purpose of the test.

### **3. Mocks Should Be Used for External Dependencies**

The purpose of mocking is to isolate dependencies. In `sendFriendRequest`, the dependency is `UserRepository` (since it interacts with a database), not `User` itself.  
We **mock `UserRepository`** to:

- Avoid real database calls.
- Control test conditions by simulating different repository responses.

But **`User` is part of the logic being tested**, so we use real objects.

---

### **When Would You Mock `User`?**

You’d consider mocking `User` if:

- It had complex behavior inside (e.g., database calls in its methods).
- It relied on other dependencies (e.g., a `NotificationService` inside `User`).
- You wanted to isolate a different part of the logic that depended on `User`.

However, for a simple data class like `User`, it's better to use real instances.

---

### **TL;DR**

Even in **unit tests**, we don't mock simple objects whose state changes we need to verify. We only mock dependencies like repositories, services, or external APIs.

Let me know if that clears things up! 🚀

# Keeping Tests Dry

I wanted to extract the shared logic that checks for exceptions being thrown when a user isn't found.

The solutions are:
- Helper method - almost the same amount of code
- Create a base test class and extend from it - similarly not much of a DRY benefit
- JUnit 5's parameterized tests - too complicated, makes tests less clear

# Why put @Valid on the fullName field of User

Because It can check everything inside the fullName (as there are validation annotations on the fields in that class).

@Valid comes into play after @NotEmpty if fullName isn't null.

# @Transactional - Keeping operations tied together

Spring’s `@Transactional` makes sure that **all DB changes within the method are wrapped in one atomic operation**.  
If anything **fails mid-way**, the database **undoes** all the changes, preventing half-saved states.

# Minimal Testing Approach

- **Unit Tests**:
    
    - **Service layer** (business logic).
    - **Utility classes**.
    - **Security filters** (authentication/authorization).
- **Integration Tests**:

    - **Controller integration** (with service and repository integration).
    - **Security integration** (testing authentication and authorization).
- **Optional**:
    
    - **End-to-end tests** (only if critical user flows need testing).

### What to Skip:

- **Unit tests for Controllers**: Since you're already testing the controller's functionality via integration tests, unit tests for controllers themselves are redundant.
- **Repository unit tests**: These are generally not necessary unless your repositories contain custom queries that require testing.
- **Mocking controllers in integration tests**: In integration tests, mock only dependencies (e.g., services or repositories), not the controllers themselves.

This streamlined approach should allow you to test the core functionality of your app with minimal effort while covering the most critical parts of the system.