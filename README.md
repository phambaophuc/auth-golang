# 🔐 Auth-Golang: A Robust Authentication and Authorization System in Go

[![Go](https://img.shields.io/badge/Go-1.23.6-blue.svg)](https://golang.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

This project provides a comprehensive authentication and authorization system built with Go, adhering to SOLID principles and Clean Architecture. It's designed to be a secure and scalable foundation for managing user identities and access control in your applications.

## 📝 Table of Contents

*   [About the Project](#about-the-project)
*   [Features](#features-)
*   [Tech Stack](#tech-stack-)
*   [Project Structure](#project-structure-)
*   [Installation](#installation-)
*   [Usage](#usage-)
*   [SOLID Principles Applied](#solid-principles-applied-)
*   [License](#license-)
*   [Footer](#footer-)

## ℹ️ About the Project

The `auth-golang` project is a Go-based backend service focused on handling user authentication and authorization. It implements a layered architecture following Clean Architecture principles, ensuring maintainability, testability, and adherence to software design best practices.

## ✨ Features

This system offers a robust set of features for managing user authentication and authorization:

### Authentication

*   `✅` **User Registration**: Securely register new users with email and password.
*   `✅` **Login**: Authenticate users using their email and password.
*   `✅` **JWT Tokens**: Issue short-lived access tokens and long-lived refresh tokens for secure session management.
*   `✅` **Token Refresh**: Seamlessly refresh access tokens using valid refresh tokens.
*   `✅` **Logout**: Basic logout functionality (token blacklisting could be added for production).

### Authorization

*   `✅` **Role-Based Access Control (RBAC)**: Assign roles to users for group-level permissions.
*   `✅` **Permission-Based Authorization**: Define fine-grained permissions for specific actions on resources.
*   `✅` **Authentication Middleware**: Protect API endpoints requiring a valid access token.
*   `✅` **Permission Middleware**: Enforce access control based on user roles and permissions.

### User Management

*   `✅` **View User Profile**: Retrieve detailed user information.
*   `✅` **Update Profile**: Allow users to update their first and last names.
*   `✅` **Change Password**: Securely update a user's password.
*   `✅` **Role Management (Admin)**: Admins can assign and remove roles from users.

## 🛠️ Tech Stack

*   **Language**: Go (Golang)
*   **Web Framework**: Gin (High-performance, minimalist web framework)
*   **Database**: PostgreSQL (Relational database)
*   **ORM**: GORM (Powerful Go ORM for PostgreSQL)
*   **Authentication**: JWT (JSON Web Tokens)
*   **Password Hashing**: bcrypt (Secure password hashing)
*   **Configuration**: `godotenv` (Environment variable loading)
*   **Dependencies**: Managed via Go Modules (`go.mod`)

## 📁 Project Structure

The project follows a Clean Architecture approach, organizing code into logical layers:

```
auth-system/
├── cmd/server/          # Application entry point
├── internal/
│   ├── config/          # Configuration management (env loading)
│   ├── domain/          # Core business entities & interfaces (repositories, services)
│   │   ├── entities/      # Data models (User, Role, Permission)
│   │   ├── repositories/  # Repository interfaces
│   │   └── services/      # Service interfaces
│   ├── application/     # Application logic (use cases, DTOs, concrete services)
│   │   ├── dto/           # Data Transfer Objects for API requests/responses
│   │   └── services/      # Concrete service implementations
│   ├── infrastructure/  # External dependencies and implementations
│   │   ├── database/      # Database connection and migration logic
│   │   └── security/      # Security implementations (JWT, Password hashing)
│   └── interfaces/      # Presentation layer (HTTP handlers, middleware, routes)
│       ├── handlers/      # HTTP request handlers
│       ├── middleware/    # Request middleware (Auth, CORS, Permissions)
│       └── routes/        # API route definitions
├── pkg/                 # Reusable utility packages (errors, response helpers)
└── .env.example        # Environment variable template
```

## 🚀 Installation

Follow these steps to set up the project locally:

1.  **Clone the repository**:
    ```bash
    git clone https://github.com/phambaophuc/auth-golang.git
    cd auth-golang
    ```

2.  **Install Go dependencies**:
    ```bash
    go mod download
    ```

3.  **Configure Environment Variables**:
    *   Copy the example environment file:
        ```bash
        cp .env.example .env
        ```
    *   Edit the `.env` file and update the following settings:
        *   **Database Configuration**: `DB_HOST`, `DB_PORT`, `DB_USER`, `DB_PASSWORD`, `DB_NAME`, `DB_SSL_MODE`.
        *   **JWT Configuration**: `JWT_SECRET`, `JWT_ACCESS_TTL`, `JWT_REFRESH_TTL` (ensure `JWT_SECRET` is strong and unique for production).
        *   **Server Configuration**: `SERVER_PORT` (defaults to 8080).

4.  **Set up PostgreSQL Database**:
    *   Ensure you have PostgreSQL installed and running.
    *   Create the database specified in your `.env` file (e.g., `auth_db`).
        ```bash
        # Example using psql command line
        createdb auth_db
        ```

## ▶️ Usage

### Running the Application

Once dependencies are installed and the environment is configured, start the server:

```bash
go run cmd/server/main.go
```

The application will start and listen on the configured `SERVER_PORT` (default is 8080).

### Real-world Use Cases

This authentication and authorization system is suitable for:

*   **Microservices Backend**: Providing a centralized authentication and authorization service for multiple microservices.
*   **Web Applications**: Securing user accounts and controlling access to different parts of a web application.
*   **Mobile App Backends**: Managing user authentication and API access for mobile clients.
*   **Internal Tools**: Restricting access to sensitive internal company resources.

## 🔑 How to Use

1.  **Authentication**: Users first register or log in to obtain JWT tokens.
2.  **Authorization**: Access protected endpoints by including the `Authorization` header with the Bearer token.
3.  **Admin Actions**: Administrative tasks like role assignment require specific permissions, enforced by middleware.

## 🧪 Testing with `curl`

Here are examples of how to interact with the API using `curl`:

#### Register a new user:

```bash
curl -X POST http://localhost:8080/api/v1/auth/register \
  -H "Content-Type: application/json" \
  -d '{
    "email": "test@example.com",
    "password": "password123",
    "first_name": "Test",
    "last_name": "User"
  }'
```

#### Log in a user:

```bash
curl -X POST http://localhost:8080/api/v1/auth/login \
  -H "Content-Type: application/json" \
  -d '{
    "email": "test@example.com",
    "password": "password123"
  }'
```

#### Get user profile (requires a valid access token):

```bash
curl -X GET http://localhost:8080/api/v1/user/profile \
  -H "Authorization: Bearer <your_access_token>"
```

## 🔧 SOLID Principles Applied

This project is built with SOLID principles in mind:

1.  **Single Responsibility Principle (SRP)**: Each struct and package has a single, well-defined responsibility.
2.  **Open/Closed Principle (OCP)**: Interfaces are used extensively, allowing for extension of functionality without modifying existing code.
3.  **Liskov Substitution Principle (LSP)**: Implementations of interfaces can be substituted for their interface types without altering the correctness of the program.
4.  **Interface Segregation Principle (ISP)**: Interfaces are kept small and focused, ensuring clients only depend on methods they need.
5.  **Dependency Inversion Principle (DIP)**: High-level modules do not depend on low-level modules; both depend on abstractions.

## 📄 License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details.

## 🌟 Footer

--- 

**auth-golang** | [phambaophuc/auth-golang](https://github.com/phambaophuc/auth-golang) 

*   Author: [Phambaophuc](https://github.com/phambaophuc)

If you find this project useful, please consider giving it a star ⭐, forking it 🍴, or opening an issue to report bugs or suggest improvements.


---
