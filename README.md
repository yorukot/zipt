# Zipt - URL Shortener

A lightweight, high-performance URL shortener built with Go's Gin framework. Zipt provides a robust solution for creating and managing shortened URLs with custom slugs, expiration dates, and analytics.

![Go Version](https://img.shields.io/badge/Go-v1.22+-blue.svg)
![Gin Version](https://img.shields.io/badge/Gin-v1.10.0-green.svg)
![License](https://img.shields.io/badge/License-MIT-yellow.svg)

## Features

- **URL Shortening**: Create short links with random codes or custom slugs
- **Link Expiration**: Set optional expiration dates for links
- **Modern Go Architecture**: Clean, modular structure for maintainable code
- **Multiple Database Support**: PostgreSQL, MySQL/MariaDB, and SQLite
- **Authentication**: JWT-based authentication with secure cookie storage
- **User Management**: Complete user registration and profile management
- **Logging**: Structured logging with levels using Zap
- **Middleware**: Request logging, error handling, and JWT validation
- **Environment Configuration**: Flexible configuration via environment variables
- **Docker Support**: Ready-to-use Docker and Docker Compose configurations
- **Caching**: Redis integration for performance optimization
- **Security**: Password hashing with Argon2, JWT token management
- **OAuth**: Optional social login integration (Google, GitHub, GitLab)
- **Object Storage**: Optional S3-compatible storage integration
- **Email**: Optional SMTP support for sending emails

## Project Structure

```
.
├── app/                       # Application code
│   ├── controllers/           # HTTP request handlers
│   │   ├── auth/              # Authentication controllers (login, signup)
│   │   ├── shortener/         # URL shortener controllers
│   │   ├── user/              # User-related controllers (profile)
│   │   └── ...                # Add any other necessary controllers
│   ├── models/                # Database models
│   ├── queries/               # Database query functions
│   └── routes/                # Route definitions
├── pkg/                       # Reusable packages
│   ├── cache/                 # Redis cache implementation
│   ├── database/              # Database connection and utilities
│   ├── encryption/            # JWT and password encryption
│   ├── logger/                # Logging configuration
│   ├── middleware/            # Gin middleware (auth, logging, error handling)
│   ├── oauth/                 # OAuth providers integration
│   ├── s3/                    # S3 storage integration
│   └── utils/                 # Utility functions and error codes
├── static/                    # Static files (favicon, etc.)
├── .env                       # Environment variables (local development)
├── template.env               # Environment template (for deployment)
├── docker-compose.yml         # Docker Compose configuration
├── Dockerfile                 # Docker build configuration
├── go.mod                     # Go modules definition
├── go.sum                     # Go modules checksums
├── main.go                    # Application entry point
└── README.md                  # Project documentation
```

## Prerequisites

- [Go](https://golang.org/doc/install) (version 1.22 or higher)
- [Docker](https://docs.docker.com/get-docker/) and [Docker Compose](https://docs.docker.com/compose/install/) (for containerized deployment)

## Getting Started

### Local Development

1. **Clone the repository:**
   ```bash
   git clone https://github.com/yorukot/zipt
   ```

2. **Navigate to the project directory:**
   ```bash
   cd zipt
   ```

3. **Setup the environment:**
   ```bash
   cp template.env .env
   ```
   Edit the `.env` file to configure your local development environment.

4. **Install dependencies:**
   ```bash
   go mod tidy
   ```

5. **Run the application:**
   ```bash
   go run main.go
   ```

6. **Access the API:**
   The API will be available at `http://localhost:8080/api/v1`

### API Endpoints

#### URL Shortener

- **POST /api/v1/urls**: Create a shortened URL
  ```json
  {
    "original_url": "https://example.com/very/long/path/to/something",
    "custom_slug": "my-custom-slug",  // Optional
    "expires_at": "2023-12-31T23:59:59Z"  // Optional
  }
  ```
  Response:
  ```json
  {
    "status": "success",
    "message": "Short URL created successfully",
    "data": {
      "short_code": "my-custom-slug",
      "original_url": "https://example.com/very/long/path/to/something",
      "short_url": "http://yourdomain.com/my-custom-slug",
      "expires_at": "2023-12-31T23:59:59Z",
      "created_at": "2023-06-01T12:34:56Z"
    }
  }
  ```

#### Authentication

- **POST /api/v1/auth/signup**: Register a new user
  ```json
  {
    "display_name": "John Doe",
    "email": "john@example.com",
    "password": "secure_password"
  }
  ```

- **POST /api/v1/auth/login**: Login with email and password
  ```json
  {
    "email": "john@example.com",
    "password": "secure_password"
  }
  ```

#### User Management

- **GET /api/v1/user/profile**: Get current user profile (requires authentication)

## Docker Deployment

### Running with Docker Compose

This project includes Docker and Docker Compose configurations for easy deployment.

```bash
docker compose up -d
```

This command starts:
- The application (running on port 8080)
- PostgreSQL database (running on port 5432)
- Redis cache (running on port 6379)

### Database Configuration

The PostgreSQL database is configured with:
- User: zipt
- Password: xxxxxxxxxx (change in production)
- Database: zipt
- Host: postgres
- Port: 5432

### Data Persistence

All database data is stored in Docker volumes to ensure persistence:
- PostgreSQL: postgres-data
- Redis: redis-data

### Stopping the Application

```bash
docker compose down
```

To remove volumes (this will delete all data):
```bash
docker compose down -v
```

## Configuration

The application is configured through environment variables. See `template.env` for all available options:

### Core Settings
- `GIN_MODE`: Set to `debug` for development, `release` for production
- `PORT`: The port the application listens on (default: 8080)
- `VERSION`: API version
- `BASE_URL`: Base URL for the application (used for generating short links)

### Database Settings
- `DATABASE_TYPE`: Database type (`postgres`, `mysql`, `mariadb`, `sqlite`)
- Database connection parameters for each supported database

### Security
- `JWT_SECRET_KEY`: Secret key for JWT token signing (change in production)
- `COOKIE_DOMAIN`: Domain for cookies
- `COOKIE_REFRESH_TOKEN_EXPIRES`: Refresh token expiration (days)
- `COOKIE_ACCESS_TOKEN_EXPIRES`: Access token expiration (minutes)

### Optional Features
- Redis cache settings
- S3 storage settings
- OAuth provider settings
- SMTP settings for email

## License

This project is licensed under the MIT License - see the LICENSE file for details.
