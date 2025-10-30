# SpringBoot_Lesson4

## Propmt for the Code Agent (Codex, Gemini Code Assistant or Copilot)

**Context**:

You are an AI assistant that helps refactor Spring Boot projects.

We are evolving our Task API from Lesson 3. We will replace the in-memory List with a persistent data layer using Spring Data JPA and the H2 in-memory database.

**Task**:

Refactor the project to use a database for storing tasks.

**Constraints**:

Spring Boot version: 3.3.0, Java 17, Maven.

The controller will talk directly to the repository for this lesson.

**Steps**:

Update the pom.xml to add two new dependencies: spring-boot-starter-data-jpa and com.h2database (with runtime scope).

Create a new file src/main/resources/application.properties and add the following lines to enable the H2 console and configure the database:

spring.h2.console.enabled=true

spring.datasource.url=jdbc:h2:mem:testdb

spring.datasource.driverClassName=org.h2.Driver

spring.datasource.username=sa

spring.datasource.password=

spring.jpa.database-platform=org.hibernate.dialect.H2Dialect

Convert the Task.java record into a class.

Annotate it with @Entity.

The id field should be annotated with @Id and @GeneratedValue(strategy = GenerationType.IDENTITY).

The other fields (description, completed) remain the same.

Add a protected no-argument constructor.

Add a constructor for creating new tasks (without the ID).

Add standard getters and setters for all fields.

Create a new Java interface TaskRepository.java.

It should extend JpaRepository<Task, Long>.

No methods need to be added to the interface body.

Refactor TaskController.java:

Remove the in-memory List<Task> and the ID counter.

Inject the TaskRepository using constructor injection.

Update the GET /tasks method to call taskRepository.findAll().

Update the POST /tasks method to call taskRepository.save(). Note that you will need to create a new Task entity from the incoming request data. The request body might be a simpler object without an ID. For this exercise, let's assume the request body maps to a Task object, and we save it directly.

Update the GET /tasks/{id} method to call taskRepository.findById(id). This returns an Optional<Task>, which is a better way to handle "not found" cases.

Provide curl commands to test the API. The commands are the same as before, but the behavior should demonstrate persistence.

**Deliverables**:

Updated pom.xml.

src/main/resources/application.properties file.

Updated src/main/java/com/example/demo/Task.java entity class.

src/main/java/com/example/demo/TaskRepository.java interface.

Updated src/main/java/com/example/demo/TaskController.java.

curl commands for testing.

**Acceptance Criteria**:

The application compiles and runs.

You can access the H2 console in your browser at http://localhost:8080/h2-console. Use the JDBC URL jdbc:h2:mem:testdb. You should be able to see an empty TASK table.

POST /tasks with a JSON body adds a row to the TASK table in the database and returns the created task.

GET /tasks returns a JSON array of all tasks stored in the database.

If you restart the application, all data is gone (because H2 is in-memory). This is expected. We will address connecting to a permanent database later.
