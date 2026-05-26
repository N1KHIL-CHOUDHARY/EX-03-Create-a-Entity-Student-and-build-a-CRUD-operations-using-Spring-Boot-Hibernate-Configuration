# Exp_03_-Entity-Student-and-build-a-CRUD-operations-using-Spring-Boot-Hibernate-Configuration

## AIM:
To develop a Spring Boot application that performs CRUD (Create, Read, Update, Delete) operations on a Student entity using Spring Data JPA (Hibernate).

## ALGORITHM:
Create Spring Boot Project

Add dependencies: Spring Web, Spring Data JPA, H2 Database or MySQL, Spring Boot DevTools

Configure application.properties

Define database connection

Enable Hibernate auto DDL

Create Student Entity Class

Annotate with @Entity

Define fields with @Id, @GeneratedValue, etc.

Create StudentRepository

Extend JpaRepository<Student, Long> for CRUD methods

Create StudentController

Handle HTTP methods:

POST /students → Add student

GET /students → Get all students

GET /students/{id} → Get student by ID

PUT /students/{id} → Update student

DELETE /students/{id} → Delete student

##PROGRAM CODE

### pom.xml
```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
         https://maven.apache.org/xsd/maven-4.0.0.xsd">

	<modelVersion>4.0.0</modelVersion>

	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>3.5.0</version>
		<relativePath/>
	</parent>

	<groupId>com.example</groupId>
	<artifactId>orm</artifactId>
	<version>0.0.1-SNAPSHOT</version>

	<properties>
		<java.version>17</java.version>
	</properties>

	<dependencies>

		<!-- Spring Web -->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>

		<!-- Spring Data JPA -->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-data-jpa</artifactId>
		</dependency>

		<!-- H2 Database -->
		<dependency>
			<groupId>com.h2database</groupId>
			<artifactId>h2</artifactId>
			<scope>runtime</scope>
		</dependency>

		<!-- Lombok -->
		<dependency>
			<groupId>org.projectlombok</groupId>
			<artifactId>lombok</artifactId>
			<optional>true</optional>
		</dependency>

		<!-- Testing -->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>

	</dependencies>

	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
		</plugins>
	</build>

</project>

```

 ### application.properties
 ```
spring.application.name=CRUD

spring.datasource.url=jdbc:h2:mem:testdb
spring.datasource.driverClassName=org.h2.Driver
spring.datasource.username=sa
spring.datasource.password=

spring.jpa.database-platform=org.hibernate.dialect.H2Dialect
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true

spring.h2.console.enabled=true
spring.h2.console.path=/h2-console

```
### Student.java

```
package com.example.CRUD;

import jakarta.persistence.Entity;
import jakarta.persistence.Id;

@Entity
public class Student {
    @Id
    private int id;
    private String name;
    private String department;

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getDepartment() {
        return department;
    }

    public void setDepartment(String department) {
        this.department = department;
    }
}


```

### StudentRepository.java

```
package com.example.CRUD;

import org.springframework.data.jpa.repository.JpaRepository;

public interface StudentRepository extends JpaRepository<Student, Integer>{

}

```


### StudentService.java

```
package com.example.CRUD;

import org.springframework.stereotype.Service;
import java.util.List;

@Service
public class StudentService {

    private final StudentRepository repo;

    public StudentService(StudentRepository repo) {
        this.repo = repo;
    }

    public Student saveStudent(Student s) {
        return repo.save(s);
    }

    public List<Student> getAllStudents() {
        return repo.findAll();
    }

    public Student updateStudent(int id, Student s) {
        Student data = repo.findById(id).orElse(null);

        if (data != null) {
            data.setName(s.getName());
            data.setId(s.getId());
            data.setDepartment(s.getDepartment());
            return repo.save(data);
        }

        return null;
    }

    public String deleteStudent(int id) {
        repo.deleteById(id);
        return "Deleted Successfully";
    }
}

```

### CRUDApplication.java
```
package com.example.CRUD;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class CrudApplication {

	public static void main(String[] args) {
		SpringApplication.run(CrudApplication.class, args);
	}

}


```

### OUTPUT:

## GET (localhost:8080/all):
<img width="1912" height="1190" alt="Screenshot 2026-05-20 at 11 29 31 AM" src="https://github.com/user-attachments/assets/feb47e14-c78f-4113-8b1c-ee2fecb8e20b" />

## POST (localhost:8080/save):
<img width="1912" height="1190" alt="Screenshot 2026-05-20 at 11 29 50 AM" src="https://github.com/user-attachments/assets/d01758e2-6cd1-4544-804b-34829de2e64a" />

## PUT (localhost:8080/{id}):
<img width="1912" height="1190" alt="Screenshot 2026-05-20 at 11 29 23 AM" src="https://github.com/user-attachments/assets/d6c81e4f-0290-4a35-b0aa-cdeaa77c3c69" />

## DELETE (localhost:8080/{id}):
<img width="1912" height="1190" alt="Screenshot 2026-05-20 at 11 30 04 AM" src="https://github.com/user-attachments/assets/f67ca0f9-bac6-4320-a5ec-afdf326a6407" />


