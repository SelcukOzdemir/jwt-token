# JWT Authentication with Refresh Token  
Spring Boot + MySQL + Docker

## Overview

This project implements a stateless JWT-based authentication system using Spring Boot, featuring both access and refresh tokens. It secures API endpoints with Spring Security and leverages a MySQL database for persistent storage of users and tokens. Docker Compose is used to simplify environment setup.

## Features

- Access and refresh token generation  
- JWT-based stateless authentication  
- Token expiration verification and renewal  
- Password hashing with BCrypt  
- Role-based access control (RBAC)  
- Stateless API secured via Spring Security  
- Custom `UserDetailsService` implementation  
- Exception handling for authentication and validation  
- Dockerized MySQL for local development

## Technologies Used

- Java 17  
- Spring Boot  
- Spring Security  
- JWT (io.jsonwebtoken - jjwt)  
- MySQL  
- Docker & Docker Compose  
- Lombok  
- Maven

## Project Structure

- `/auth/register`: Register new users  
- `/auth/login`: Authenticate users and issue access & refresh tokens  
- `/auth/refresh`: Generate a new access token using a valid refresh token  
- `/api/secure`: A protected endpoint accessible only to authenticated users with `ROLE_USER`

## Getting Started

### Prerequisites

- Docker & Docker Compose  
- Java 17+  
- Maven

### 1. Clone the repository

```bash
git clone https://github.com/your-username/jwt-auth-refresh-token.git
cd jwt-auth-refresh-token
```

### 2. Set up MySQL with Docker

```yaml
# docker-compose.yml
services:
  db:
    image: mysql
    container_name: sso_db
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: sso_db
      MYSQL_USER: sso_user
      MYSQL_PASSWORD: sso_pass
    ports:
      - "3306:3306"
```

Run:

```bash
docker-compose up -d
```

### 3. Configure `application.properties`

```properties
spring.datasource.url=jdbc:mysql://localhost:3306/sso_db
spring.datasource.username=sso_user
spring.datasource.password=sso_pass
spring.jpa.hibernate.ddl-auto=update

jwt_token=your-secret-key
```

### 4. Run the application

```bash
./mvnw spring-boot:run
```

## API Endpoints

### 1. Register

`POST /auth/register`

Request:

```json
{
  "username": "john",
  "password": "password123"
}
```

### 2. Login

`POST /auth/login`

Request:

```json
{
  "username": "john",
  "password": "password123"
}
```

Response:

```json
{
  "accessToken": "jwt-token",
  "refreshToken": "refresh-token"
}
```

### 3. Refresh Token

`POST /auth/refresh`

Request:

```json
{
  "refreshToken": "your-refresh-token"
}
```

Response:

```json
{
  "accessToken": "new-jwt-token",
  "refreshToken": "your-refresh-token"
}
```

### 4. Secure Endpoint

`GET /api/secure`

Header:

```
Authorization: Bearer <accessToken>
```

Response:

```
Hello, authenticated user!
```
