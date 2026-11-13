# University of Moratuwa - Faculty of Information Technology
## IN3101: Enterprise Application Development
### Lab Sheet 02 — Building a REST API with Spring Boot

**Student Name:** Vibhushan Ganeshan  
**Index Number:** 235115D  
**Academic Year:** 2026  

---

## 1. Project Overview

This repository contains the complete implementation of **Lab Sheet 02 - Building a REST API with Spring Boot** for the module **IN3101: Enterprise Application Development**.

The application is a standalone RESTful Web Service managing a library catalog of books (`Book` entity), following the layered architecture:
- **Model / Entity Layer:** JPA entity `Book` with bean validations.
- **Data Access Layer:** Spring Data JPA repository `BookRepository`.
- **Business Logic Layer:** Service layer `BookService` with business operations.
- **Presentation / API Layer:** REST controller `BookController` exposing standard HTTP endpoints.
- **API Documentation:** Interactive OpenAPI 3 / Swagger UI documentation.

---

## 2. Technologies Used

- **Java:** JDK 21 (LTS)
- **Framework:** Spring Boot 3.4.5 (Spring Framework 6.2, Spring MVC, Spring Data JPA)
- **Database:** MySQL Server 8.0
- **Validation:** Jakarta Validation (`@NotBlank`, `@PositiveOrZero`)
- **API Documentation:** springdoc-openapi 2.8.5 (`swagger-ui`)
- **Build Tool:** Apache Maven with Maven Wrapper (`mvnw`)
- **Testing:** JUnit 5, Spring Boot Test, Postman

---

## 3. Database Configuration

### 3.1 Database Setup

Create the database in MySQL Server on port `3306`:

```sql
CREATE DATABASE IF NOT EXISTS library_db;
```

### 3.2 Application Properties (`src/main/resources/application.properties`)

```properties
spring.application.name=library-api
spring.datasource.url=jdbc:mysql://localhost:3306/library_db
spring.datasource.username=root
spring.datasource.password=${MYSQL_PASSWORD}
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
```

> **Security Note:** The database password is provided securely via the environment variable `MYSQL_PASSWORD` and is never committed to the repository.

---

## 4. Build and Run Instructions

### 4.1 Prerequisites
- Java JDK 21 installed and configured in `PATH`
- MySQL Server 8.0 running on `localhost:3306`
- Set the `MYSQL_PASSWORD` environment variable in your terminal:
  - **PowerShell:** `$env:MYSQL_PASSWORD="your_password"`
  - **CMD:** `set MYSQL_PASSWORD=your_password`
  - **Bash:** `export MYSQL_PASSWORD=your_password`

### 4.2 Run Tests
```powershell
.\mvnw.cmd clean test
```

### 4.3 Run the Application
```powershell
.\mvnw.cmd spring-boot:run
```

The application starts by default on port `8080`: `http://localhost:8080`.

---

## 5. REST API Endpoints

Base path: `/api/books`

| Method | Endpoint | Description | Request Body | Response Codes |
|---|---|---|---|---|
| **GET** | `/api/books` | Retrieve list of all books | None | `200 OK` |
| **GET** | `/api/books/{id}` | Retrieve single book by ID | None | `200 OK`, `404 Not Found` |
| **POST** | `/api/books` | Create a new book | JSON (`Book`) | `201 Created`, `400 Bad Request` |
| **PUT** | `/api/books/{id}` | Update existing book by ID | JSON (`Book`) | `200 OK`, `404 Not Found` |
| **DELETE** | `/api/books/{id}` | Delete book by ID | None | `204 No Content`, `404 Not Found` |

### Sample JSON Request Body (POST / PUT)
```json
{
  "title": "235115D - Clean Code",
  "author": "Robert C. Martin",
  "isbn": "9780132350884",
  "price": 4500.00
}
```

---

## 6. Swagger / OpenAPI Documentation

Interactive Swagger documentation is available once the application is running:

- **Swagger UI:** [http://localhost:8080/swagger-ui.html](http://localhost:8080/swagger-ui.html)
- **OpenAPI v3 JSON:** [http://localhost:8080/v3/api-docs](http://localhost:8080/v3/api-docs)

All 5 endpoints are documented under the **Books** tag with detailed response codes (including `201 Created` and `400 Bad Request` on `POST /api/books`).

---

## 7. Postman Testing Instructions

A ready-to-use Postman Collection is provided at:
- `postman/IN3101_Lab02_Book_API.postman_collection.json`

### Import & Test Execution
1. Open Postman.
2. Click **Import** and select `IN3101_Lab02_Book_API.postman_collection.json`.
3. The collection is configured with seven test scenarios matching the lab requirements:
   - **S1:** `POST /api/books` -> `201 Created`
   - **S2:** `GET /api/books` -> `200 OK` (verifies at least 3 books and student index `235115D`)
   - **S3:** `GET /api/books/{id}` -> `200 OK`
   - **S4:** `PUT /api/books/{id}` -> `200 OK`
   - **S5:** `DELETE /api/books/{id}` -> `204 No Content`
   - **S6:** `GET /api/books/{id}` -> `404 Not Found` (verifies deletion of the same ID)
   - **S7:** `POST /api/books` -> `400 Bad Request` (input validation with blank title)

---

## 8. Student Verification

- **Student:** Vibhushan Ganeshan
- **Index:** 235115D
- **University:** University of Moratuwa
- **Faculty:** Faculty of Information Technology

---

## 9. Lab Sheet 03 â€” Containerising the Library API with Docker

### 9.1 Architecture
- **API Service (`api`):** Spring Boot 3.4.5 built with Eclipse Temurin Java 21 JRE, exposed on port `8080`.
- **Database Service (`db`):** MySQL 8.4 Server exposed on host port `3307` (container port `3306`) with health check.
- **Storage:** Named volume `library-data` mapped to `/var/lib/mysql` for persistent database storage across container lifecycles.
- **Orchestration:** Managed using Docker Compose (`compose.yaml`).

### 9.2 Running with Docker Compose

1. **Configure Environment:**
   Copy `.env.example` to `.env` (or configure local password):
   ```bash
   cp .env.example .env
   # Ensure DB_ROOT_PASSWORD is set (e.g., DB_ROOT_PASSWORD=root123)
   ```

2. **Build and Start Containers:**
   ```bash
   docker compose up --build -d
   ```

3. **Verify Service Status:**
   ```bash
   docker compose ps
   docker compose logs -f api
   ```

4. **Access the Application:**
   - **REST API:** [http://localhost:8080/api/books](http://localhost:8080/api/books)
   - **Swagger UI:** [http://localhost:8080/swagger-ui.html](http://localhost:8080/swagger-ui.html)
   - **MySQL Host Port:** `localhost:3307` (`library_db`)

5. **Stop Containers (preserving data volume):**
   ```bash
   docker compose down
   ```
   > **Note:** Never use `-v` flag if you want to preserve database records stored in the `library-data` volume.


