# Spring Boot User Registration Project

Implements the lab task: configure Spring Data JPA with PostgreSQL, create a user registration REST API, and test it using Swagger UI.

## Requirements
- Java 17 or newer
- Maven 3.9+
- PostgreSQL

## 1. Create the database
Open PostgreSQL (or pgAdmin Query Tool) and run:

```sql
CREATE DATABASE user_registration;
```

## 2. Configure the database connection
Default settings in `src/main/resources/application.properties` are:
- Database: `user_registration`
- Username: `postgres`
- Password: `postgres`

Change the username/password to match your local PostgreSQL installation, or set environment variables:
- `DB_URL`
- `DB_USERNAME`
- `DB_PASSWORD`

## 3. Run the project
From this project folder, run:

```bash
mvn spring-boot:run
```

Spring Boot creates the `users` table automatically. The password is stored as a BCrypt hash, not as plain text.

## 4. Test in Swagger UI
Open: http://localhost:8080/swagger-ui.html

Choose `POST /api/users/register`, click **Try it out**, and submit:

```json
{
  "username": "jahnavi123",
  "email": "jahnavi@example.com",
  "password": "StrongPass123"
}
```

Expected response (the generated ID may differ):

```json
{
  "id": 1,
  "username": "jahnavi123",
  "email": "jahnavi@example.com",
  "message": "User registered successfully"
}
```

The response intentionally does not return the password or password hash.

## Endpoint
- `POST /api/users/register` — validates input, checks duplicate username/email, hashes the password, and saves the user.

## Notes
- `spring.jpa.hibernate.ddl-auto=update` is convenient for a lab/demo. For production, use database migrations and external secret management.
- Duplicate username/email returns HTTP 409.
- Invalid input returns HTTP 400.
