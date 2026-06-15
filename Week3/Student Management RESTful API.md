# Recording

https://xiao-java-05182026-demo-bucket.s3.us-west-2.amazonaws.com/mock-interview/2026-06-15-11-41-23.mp4?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAW3MEATYNQS2XOAZB%2F20260615%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20260615T193435Z&X-Amz-Expires=604800&X-Amz-SignedHeaders=host&X-Amz-Signature=02ca774c7b2c8d99647dfc8b6531ebd7eb43fb1fbb04379f130e7d10b7000c90



# Clarification

Hello, today I am going to build a Student Management RESTful API from scratch using Spring Boot.

The business requirement is that, this application allows a client to create a student, retrieve a student, retrieve a list of students, update a student, and delete a student.

**First, lets design the RESTful Endpoints Before Coding**

For restful apis, we use url represent resources, and use HTTP methods represent actions

```
POST   /api/students
Success: 201 Created
Errors: 400 Bad Request, 409 Conflict

GET    /api/students/{id}
Success: 200 OK
Error: 404 Not Found

GET    /api/students?major=Computer Science
Success: 200 OK
Response: student list or empty array

PUT    /api/students/{id}
Success: 200 OK
Errors: 400 Bad Request, 404 Not Found, 409 Conflict

DELETE /api/students/{id}
Success: 204 No Content
Error: 404 Not Found
```

POST   /api/students 

- This endpoint creates a new student.
- The student information is provided in the request body, and the request ID is provided in the request header.
- If the student is created successfully, the API returns `201 Created` with the created student in the response body.

GET    /api/students/{id} 

- This endpoint retrieves one student by ID.
- The student ID is provided as a path variable.
- If the student exists, the API returns `200 OK` with the student in the response body.
- If the student does not exist, it returns `404 Not Found`

GET    /api/students?email=

- This endpoint retrieves a collection of students.
- It also supports an optional request parameter named `email`
- If the request is successful, the API returns `200 OK` with a list of students.
- If no students match the condition, it still returns `200 OK` with an empty JSON array instead of returning `404 Not Found`.



PUT    /api/students/{id} 

- It updates an existing student.
- The student ID is provided as a path variable, and the updated student information is provided in the request body.
- If the update is successful, the API returns `200 OK` with the updated student in the response body.
- If the request body is invalid, it returns `400 Bad Request`.
- If the student does not exist, it returns `404 Not Found`.

DELETE /api/students/{id}

- This endpoint deletes an existing student.
- The student ID is provided as a path variable.
- If the student is deleted successfully, the API returns `204 No Content`.
- There is no response payload because the resource has already been deleted.
- If the student does not exist, the API returns `404 Not Foun`



# Dependency

The first dependency is Spring Web.

- Spring Web gives me the Spring MVC framework and the annotations needed to create RESTful endpoints

Spring Data JPA

- Spring Data JPA allows the application to communicate with the relational database through entities and repositories.

Validation

- It allows me to validate incoming request data by using annotations such as NotBlank and Email.

The fourth dependency is H2 Database.

- H2 is a lightweight relational database.

Next is Lomobok, which is used to 



# Create the Package Structure

Under the main application package, create these packages:

```
controller
dto
entity
repository
service
exception
```



#  Configure the H2 Database

Now I am configuring the H2 database.



Open:

```
src/main/resources/application.properties
```

Type

```
spring.application.name=student-management-h2

spring.datasource.url=jdbc:h2:mem:studentdb
spring.datasource.driver-class-name=org.h2.Driver
spring.datasource.username=sa
spring.datasource.password=

spring.jpa.hibernate.ddl-auto=create-drop
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.format_sql=true

spring.h2.console.enabled=true
spring.h2.console.path=/h2-console
```



# Create the Student Entity

```java
package com.april.studentmanagementproject.entity;

import jakarta.persistence.*;
import lombok.*;

/**
 * Student entity class.
 * This class represents one student record in the students database table.
 */

// Lombok automatically generates getter methods for all fields.
@Getter

// Lombok automatically generates setter methods for all fields.
@Setter

// Lombok generates a constructor with no parameters.
@NoArgsConstructor

// Lombok generates a constructor containing all fields as parameters.
@AllArgsConstructor

// Marks this class as a JPA entity.
// JPA will map objects of this class to records in a database table.
@Entity

// Specifies that this entity is mapped to the "students" table.
@Table(name = "students")
public class Student {

    // Marks this field as the primary key of the table.
    @Id

    // Tells the database to generate the ID automatically.
    // IDENTITY usually uses the database's auto-increment feature.
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    // Maps this field to the "first_name" column.
    @Column(name = "first_name")
    private String firstName;

    // Maps this field to the "last_name" column.
    @Column(name = "last_name")
    private String lastName;

    // Maps this field to the "email_id" column.
    // nullable = false means the email cannot be null.
    // unique = true means duplicate email values are not allowed.
    @Column(name = "email_id", nullable = false, unique = true)
    private String email;
}
```



# Create the Student DTO

Next, I am creating a Student DTO.

I use DTOs as the request and response models for the REST API. They define what data the client can send to the application and what data the application returns to the client. This also helps us avoid exposing the entity directly.

```java
package com.april.studentmanagementproject.dto;

import jakarta.validation.constraints.Email;
import jakarta.validation.constraints.NotBlank;
import jakarta.validation.constraints.Size;
import lombok.AllArgsConstructor;
import lombok.Getter;
import lombok.NoArgsConstructor;
import lombok.Setter;

/**
 * StudentDto is used as the request and response model
 * for student-related REST API operations.
 */

// Lombok automatically generates getter methods for all fields.
@Getter

// Lombok automatically generates setter methods for all fields.
@Setter

// Lombok generates a constructor with no parameters.
@NoArgsConstructor

// Lombok generates a constructor containing all fields as parameters.
@AllArgsConstructor
public class StudentDto {

    private Long id;

    // Ensures that the first name is not null, empty, or only whitespace.
    @NotBlank(message = "First name is required")

    // Limits the first name to a maximum of 50 characters.
    @Size(max = 50, message = "First name must be at most 50 characters")
    private String firstName;

    // Ensures that the last name is not null, empty, or only whitespace.
    @NotBlank(message = "Last name is required")

    // Limits the last name to a maximum of 50 characters.
    @Size(max = 50, message = "Last name must be at most 50 characters")
    private String lastName;

    // Ensures that the email is not null, empty, or only whitespace.
    @NotBlank(message = "Email is required")

    // Ensures that the value follows a valid email format.
    @Email(message = "Email must be valid")

    // Limits the email address to a maximum of 100 characters.
    @Size(max = 100, message = "Email must be at most 100 characters")
    private String email;
}
```



# Create StudentMapper

Now I’m going to create the `StudentMapper`.

We need this mapper because it converts between `Student` entity and `StudentDto`.

 The entity is used for database operations, while the DTO is used for API requests and responses.

```java
package com.april.studentmanagementproject.mapper;

import com.april.studentmanagementproject.dto.StudentDto;
import com.april.studentmanagementproject.entity.Student;
import org.springframework.stereotype.Component;

import java.util.List;

/**
 * Converts data between Student entities and StudentDto objects.
 *
 * The Student entity represents the database model.
 * StudentDto represents the request and response model of the REST API.
 *
 * Keeping the mapping logic in a separate class helps keep the
 * service layer clean and avoids exposing database entities directly.
 */

// Registers this class as a Spring bean.
//
// Because it is managed by Spring, StudentMapper can be injected
// into other classes, such as StudentServiceImpl.
@Component
public class StudentMapper {

    /**
     * Converts a Student entity into a StudentDto.
     *
     * This method is usually used after reading student data
     * from the database and before returning it to the client.
     *
     * @param student the Student entity to convert
     * @return the converted StudentDto, or null if the input is null
     */
    public StudentDto toDto(Student student) {

        // Prevents a NullPointerException when the input is null.
        if (student == null) {
            return null;
        }

        // Creates a DTO using the data from the entity.
        return new StudentDto(
                student.getId(),
                student.getFirstName(),
                student.getLastName(),
                student.getEmail()
        );
    }

    /**
     * Converts a StudentDto into a Student entity.
     *
     * This method is usually used before saving student data
     * received from the client into the database.
     *
     * @param studentDto the StudentDto to convert
     * @return the converted Student entity, or null if the input is null
     */
    public Student toEntity(StudentDto studentDto) {

        // Prevents a NullPointerException when the input is null.
        if (studentDto == null) {
            return null;
        }

        // Creates a new entity and copies the DTO fields into it.
        Student student = new Student();

        student.setId(studentDto.getId());
        student.setFirstName(studentDto.getFirstName());
        student.setLastName(studentDto.getLastName());
        student.setEmail(studentDto.getEmail());

        return student;
    }

    /**
     * Converts a list of Student entities into a list of StudentDto objects.
     *
     * @param students the list of Student entities
     * @return the converted list of StudentDto objects
     */
    public List<StudentDto> toDtoList(List<Student> students) {

        // Creates a stream from the entity list.
        return students.stream()

                // Converts each Student entity into a StudentDto.
                //
                // this::toDto is a method reference.
                // It is equivalent to:
                // student -> this.toDto(student)
                .map(this::toDto)

                // Collects the converted elements into a new list.
                .toList();
    }
}
```



# Create the Repository

Now I am creating the repository layer.

StudentRepository extends JpaRepository.

The first generic type is Student, which is the entity type.

The second generic type is Long, which is the type of the primary key.

JpaRepository already provides common CRUD methods, including save, findById, findAll, existsById, and delete.

I also define `findByEmail`.

```java
package com.april.studentmanagementproject.repository;

import com.april.studentmanagementproject.entity.Student;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

import java.util.Optional;

@Repository
public interface StudentRepository extends JpaRepository<Student, Long> {

    Optional<Student> findByEmail(String email);
}

```



# Create the Custom Not-Found Exception

Next I'm going to create  a custom runtime exception. It is thrown when the application cannot find a requested resource, such as a student with a specific ID.

It extends `RuntimeException`, so it is an unchecked exception. The class provides two constructors. The first constructor accepts a custom error message. The second constructor builds a standard error message based on the resource name, field name, and field value.

```java
package com.april.studentmanagementproject.exception;

/**
 * Custom runtime exception used when a requested resource
 * cannot be found in the application.
 *
 * For example, it can be thrown when a student with
 * a specific ID does not exist.
 */
public class ResourceNotFoundException extends RuntimeException {

    /**
     * Creates the exception with a custom error message.
     *
     * The message is passed to the constructor of RuntimeException.
     *
     * @param message the error message describing why the resource was not found
     */
    public ResourceNotFoundException(String message) {
        super(message);
    }

    /**
     * Creates the exception with a standard error message
     * based on the resource name, field name, and field value.
     *
     * For example:
     * Student not found with id: '1'
     *
     * @param resourceName the name of the resource, such as "Student"
     * @param fieldName the field used to search for the resource, such as "id"
     * @param fieldValue the value of the field, such as 1
     */
    public ResourceNotFoundException(
            String resourceName,
            String fieldName,
            Object fieldValue) {

        // Formats the error message and passes it to RuntimeException.
        super(String.format(
                "%s not found with %s: '%s'",
                resourceName,
                fieldName,
                fieldValue
        ));
    }
}
```



# Create the Service Interface

Next, I am defining the service interface.

The controller will depend on this abstraction instead of directly depending on the repository. This gives us the flexibility to replace the implementation in the future without changing the code that uses the service.

```java
package com.april.studentmanagementproject.service;

import com.april.studentmanagementproject.dto.StudentDto;

import java.util.List;

public interface StudentService {

    StudentDto addStudent(StudentDto studentDto);

    List<StudentDto> getAllStudents();

    List<StudentDto> getStudents(String email);

    StudentDto getStudentById(Long id);

    StudentDto updateStudent(Long id, StudentDto studentDto);

    void deleteStudent(Long id);
}

```



# Implement the Service

Now I am implementing the service layer.

The service layer contains the business logic of the application. The controller will call the service layer, and the service layer will communicate with the repository layer.

```java
package com.april.studentmanagementproject.service.impl;

import com.april.studentmanagementproject.dto.StudentDto;
import com.april.studentmanagementproject.entity.Student;
import com.april.studentmanagementproject.exception.ResourceNotFoundException;
import com.april.studentmanagementproject.mapper.StudentMapper;
import com.april.studentmanagementproject.repository.StudentRepository;
import com.april.studentmanagementproject.service.StudentService;
import org.springframework.stereotype.Service;

import java.util.Collections;
import java.util.List;

/**
 * Implements the business logic defined in the StudentService interface.
 *
 * The controller delegates student-related operations to this service class.
 * This class communicates with the repository layer and converts data
 * between Student entities and StudentDto objects.
 */

// Registers this class as a Spring service bean.
// It also indicates that this class belongs to the service layer.
@Service
public class StudentServiceImpl implements StudentService {

    /**
     * StudentRepository is used to access and modify student data
     * in the database.
     *
     * The field is private because it should only be used inside this class.
     *
     * The field is final because this dependency is required and should not
     * be reassigned after the object is created.
     */
    private final StudentRepository studentRepository;

    /**
     * StudentMapper is used to convert between Student and StudentDto.
     *
     * The Student entity represents the database model, while StudentDto
     * represents the request and response model of the REST API.
     *
     * Using a mapper keeps the conversion logic separate from the business
     * logic and prevents the entity from being exposed directly to clients.
     */
    private final StudentMapper studentMapper;

    /**
     * Uses constructor injection to provide the required dependencies.
     *
     * Constructor injection makes the dependencies clear, allows the fields
     * to be final, supports fail-fast behavior, and makes unit testing easier.
     *
     * Since this class has only one constructor, @Autowired is not required.
     */
    public StudentServiceImpl(
            StudentRepository studentRepository,
            StudentMapper studentMapper) {

        this.studentRepository = studentRepository;
        this.studentMapper = studentMapper;
    }

    /**
     * Adds a new student.
     *
     * @param studentDto the student data received from the client
     * @return the saved student as a DTO
     */
    @Override
    public StudentDto addStudent(StudentDto studentDto) {

        // Checks whether another student already uses the same email.
        // The email must be unique in this application.
        if (studentRepository.findByEmail(studentDto.getEmail()).isPresent()) {
            throw new IllegalArgumentException(
                    "Email already exists: " + studentDto.getEmail()
            );
        }

        // Converts the request DTO into a Student entity
        // before saving it to the database.
        Student student = studentMapper.toEntity(studentDto);

        // Ensures that this operation creates a new database record.
        // The database will generate the ID automatically.
        student.setId(null);

        // Saves the entity to the database.
        Student savedStudent = studentRepository.save(student);

        // Converts the saved entity back to a DTO
        // before returning it to the controller.
        return studentMapper.toDto(savedStudent);
    }

    /**
     * Retrieves all students.
     *
     * @return a list of all students as DTOs
     */
    @Override
    public List<StudentDto> getAllStudents() {

        // findAll() returns a list of Student entities.
        // The mapper converts the entity list into a DTO list.
        return studentMapper.toDtoList(studentRepository.findAll());
    }

    /**
     * Retrieves students and optionally filters them by email.
     *
     * If the email is missing or blank, all students are returned.
     * If the email is provided, only the matching student is returned.
     *
     * @param email the optional email filter
     * @return a list of matching students
     */
    @Override
    public List<StudentDto> getStudents(String email) {

        // Returns all students when no email filter is provided.
        if (email == null || email.isBlank()) {
            return getAllStudents();
        }

        // findByEmail() returns an Optional<Student>.
        return studentRepository.findByEmail(email)

                // If a student is found, convert it to a DTO
                // and return it as a list containing one element.
                .map(student -> List.of(studentMapper.toDto(student)))

                // If no student is found, return an empty list
                // instead of returning null.
                .orElseGet(Collections::emptyList);
    }

    /**
     * Retrieves one student by ID.
     *
     * @param id the ID of the student
     * @return the matching student as a DTO
     * @throws ResourceNotFoundException if the student does not exist
     */
    @Override
    public StudentDto getStudentById(Long id) {

        // Searches for the student by ID.
        // If the student does not exist, a custom exception is thrown.
        Student student = studentRepository.findById(id)
                .orElseThrow(() ->
                        new ResourceNotFoundException("Student", "id", id)
                );

        // Converts the entity into a DTO before returning it.
        return studentMapper.toDto(student);
    }

    /**
     * Updates an existing student.
     *
     * @param id the ID of the student to update
     * @param studentDto the new student data
     * @return the updated student as a DTO
     * @throws ResourceNotFoundException if the student does not exist
     */
    @Override
    public StudentDto updateStudent(Long id, StudentDto studentDto) {

        // Finds the existing student first.
        // We update the managed entity instead of creating a completely
        // new entity, so the existing ID and database record are preserved.
        Student student = studentRepository.findById(id)
                .orElseThrow(() ->
                        new ResourceNotFoundException("Student", "id", id)
                );

        // Checks whether the new email belongs to another student.
        studentRepository.findByEmail(studentDto.getEmail())
                .ifPresent(existing -> {

                    // The same student may keep its current email.
                    // An error is thrown only when another student
                    // already uses that email.
                    if (!existing.getId().equals(id)) {
                        throw new IllegalArgumentException(
                                "Email already exists: "
                                        + studentDto.getEmail()
                        );
                    }
                });

        // Updates the fields of the existing entity
        // using the data received from the DTO.
        student.setFirstName(studentDto.getFirstName());
        student.setLastName(studentDto.getLastName());
        student.setEmail(studentDto.getEmail());

        // Saves the updated entity to the database.
        Student updatedStudent = studentRepository.save(student);

        // Converts the updated entity into a DTO.
        return studentMapper.toDto(updatedStudent);
    }

    /**
     * Deletes a student by ID.
     *
     * @param id the ID of the student to delete
     * @throws ResourceNotFoundException if the student does not exist
     */
    @Override
    public void deleteStudent(Long id) {

        // Checks whether the student exists before attempting deletion.
        // This allows the application to return a clear not-found error.
        if (!studentRepository.existsById(id)) {
            throw new ResourceNotFoundException("Student", "id", id);
        }

        // Deletes the matching student record from the database.
        studentRepository.deleteById(id);
    }
}
```



# Design and Implement the Controller

```java
package com.april.studentmanagementproject.controller;

import com.april.studentmanagementproject.dto.StudentDto;
import com.april.studentmanagementproject.service.StudentService;
import jakarta.validation.Valid;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.DeleteMapping;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.PutMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;

import java.util.List;

/**
 * REST endpoints for student CRUD operations.
 */
@RestController
@RequestMapping("/api/students")
public class StudentController {

    private final StudentService studentService;

    public StudentController(StudentService studentService) {
        this.studentService = studentService;
    }

    /** Create a new student. Returns 201 Created. */
    @PostMapping
    public ResponseEntity<StudentDto> addStudent(@Valid @RequestBody StudentDto studentDto) {
        StudentDto createdStudent = studentService.addStudent(studentDto);
        return new ResponseEntity<>(createdStudent, HttpStatus.CREATED);
    }

    /** Get all students, or filter by email. */
    @GetMapping
    public ResponseEntity<List<StudentDto>> getAllStudents(@RequestParam(required = false) String email) {
        return ResponseEntity.ok(studentService.getStudents(email));
    }

    /** Get a student by id. Returns 404 if not found. */
    @GetMapping("/{id}")
    public ResponseEntity<StudentDto> getStudentById(@PathVariable Long id) {
        return ResponseEntity.ok(studentService.getStudentById(id));
    }

    /** Update an existing student by id. */
    @PutMapping("/{id}")
    public ResponseEntity<StudentDto> updateStudent(@PathVariable Long id, @Valid @RequestBody StudentDto studentDto) {
        return ResponseEntity.ok(studentService.updateStudent(id, studentDto));
    }

    /** Delete a student by id. Returns 204 No Content. */
    @DeleteMapping("/{id}")
    public ResponseEntity<Void> deleteStudent(@PathVariable Long id) {
        studentService.deleteStudent(id);
        return ResponseEntity.noContent().build();
    }
}
```

