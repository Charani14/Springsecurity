# Spring Security JWT Demo (MySQL)

A production-style **Spring Boot REST API** implementing **JWT-based authentication and authorization** using **Spring Security**, **MySQL**, **JPA/Hibernate**, and **role-based access control (USER / ADMIN)**.

This README is aligned **exactly with the current project structure and configuration** shown in the repository.

---

## ✨ Features

* User registration & login with JWT
* Role-based authorization (USER / ADMIN)
* Secure endpoints using Spring Security filter chain
* JWT generation, validation & refresh
* Method-level security with `@PreAuthorize`
* MySQL database with Hibernate
* Global exception handling
* Swagger / OpenAPI documentation

---

## 🛠 Tech Stack

* Java 17+
* Spring Boot
* Spring Security
* JWT (jjwt)
* Spring Data JPA (Hibernate)
* MySQL
* Swagger (Springdoc OpenAPI)
* Maven

---

## 📂 Project Structure

```
com.example.security
│
├── config
│   ├── DataInitializer.java
│   ├── GlobalExceptionHandler.java
│   ├── OpenApiConfig.java
│   └── SecurityConfig.java
│
├── controller
│   ├── AuthController.java
│   ├── UserController.java
│   ├── AdminController.java
│   └── PublicController.java
│
├── dto
│   ├── AuthRequest.java
│   ├── AuthResponse.java
│   ├── RegisterRequest.java
│   ├── UserResponse.java
│   └── ErrorResponse.java
│
├── filter
│   └── JwtAuthenticationFilter.java
│
├── model
│   ├── User.java
│   └── Role.java
│
├── repository
│   └── UserRepository.java
│
├── service
│   ├── AuthService.java
│   ├── JwtService.java
│   └── UserService.java
│
└── SecurityApplication.java
```

---

## ⚙️ Configuration

### application.properties

```properties
spring.application.name=security

# Database Configuration (MySQL)
spring.datasource.url=jdbc:mysql://localhost:3306/securitydb?useSSL=false&serverTimezone=UTC
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.username=root
spring.datasource.password=root

# JPA / Hibernate
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.format_sql=true
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQLDialect

# Server
server.port=8080

# JWT
jwt.secret=YOUR_BASE64_SECRET_KEY
jwt.expiration=86400000
```

---

## 🔐 Roles & Authorization

| Role  | Access                      |
| ----- | --------------------------- |
| USER  | Own profile, public APIs    |
| ADMIN | User management, admin APIs |

> ❗ Users **cannot register directly as ADMIN**. Admin access is granted only via promotion or database initialization.

---

## 🔑 Authentication Flow (JWT)

1. User registers or logs in
2. Server generates JWT token
3. Client sends token in `Authorization` header
4. `JwtAuthenticationFilter` validates token
5. Spring Security sets authentication context

```
Authorization: Bearer <JWT_TOKEN>
```

---

## 🚀 API Endpoints

### 🔓 Public Endpoints

| Method | Endpoint           |
| ------ | ------------------ |
| GET    | /api/public/health |
| GET    | /api/public/info   |

---

### 🔐 Authentication Endpoints

| Method | Endpoint           | Description           |
| ------ | ------------------ | --------------------- |
| POST   | /api/auth/register | Register new user     |
| POST   | /api/auth/login    | Login and receive JWT |
| POST   | /api/auth/refresh  | Refresh JWT token     |

#### Register Example

```json
{
  "name": "Charani",
  "email": "charani@gmail.com",
  "password": "password123"
}
```

---

### 👤 User Endpoints (Authenticated)

| Method | Endpoint        | Access         |
| ------ | --------------- | -------------- |
| GET    | /api/users/me   | USER / ADMIN   |
| GET    | /api/users      | ADMIN          |
| GET    | /api/users/{id} | ADMIN or owner |
| DELETE | /api/users/{id} | ADMIN          |

---

### 🛡 Admin Endpoints (ADMIN only)

| Method | Endpoint                      |
| ------ | ----------------------------- |
| PATCH  | /api/admin/users/{id}/promote |
| GET    | /api/admin/dashboard          |

---

## 🧪 Testing with Postman

### 1️⃣ Register

```
POST http://localhost:8080/api/auth/register
```

### 2️⃣ Login

```
POST http://localhost:8080/api/auth/login
```

Copy the JWT token.

### 3️⃣ Authorized Requests

Add header:

```
Authorization: Bearer <JWT_TOKEN>
```

---

## 🗄 Database Schema

```sql
CREATE TABLE users (
  id BIGINT AUTO_INCREMENT PRIMARY KEY,
  name VARCHAR(100) NOT NULL,
  email VARCHAR(255) UNIQUE NOT NULL,
  password VARCHAR(255) NOT NULL,
  role VARCHAR(20) NOT NULL,
  created_at DATETIME NOT NULL,
  updated_at DATETIME
);
```

---

## 📘 Swagger API Docs

```
http://localhost:8080/swagger-ui.html
```

---

## 🔒 Security Best Practices

* BCrypt password encryption
* Stateless JWT authentication
* No direct ADMIN registration
* Method-level authorization
* Centralized exception handling

---

## 🧠 Interview Notes

* JWT vs Session-based auth
* Role vs Authority
* Why ADMIN creation is restricted
* How `JwtAuthenticationFilter` works
* Benefits of stateless security

---

## ✅ Conclusion

This project demonstrates a **secure, real-world JWT authentication system** using Spring Boot and Spring Security with MySQL, following industry standards and clean architecture.

---

⭐ Star the repository if you find it useful!
