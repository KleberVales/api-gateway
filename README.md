# API Gateway

API Gateway for a Spring Boot microservices architecture.

This project provides a single entry point for client applications and is responsible for routing requests to backend microservices, validating JWT authentication, propagating authenticated user information, and providing resilience through Circuit Breaker and fallback mechanisms.

## 🏗️ Architecture

The API Gateway acts as the entry point between clients and the microservices ecosystem.

```text
                         ┌─────────────────┐
                         │     Client      │
                         │ Web / Mobile    │
                         └────────┬────────┘
                                  │
                                  │ HTTP
                                  ▼
                     ┌────────────────────────┐
                     │      API Gateway       │
                     │                        │
                     │  Spring Cloud Gateway  │
                     │  JWT Authentication   │
                     │  Routing               │
                     │  Circuit Breaker       │
                     │  Fallback              │
                     └───────┬───────┬────────┘
                             │       │
              ┌──────────────┘       └──────────────┐
              │                                     │
              ▼                                     ▼
      ┌───────────────┐                    ┌─────────────────┐
      │  Auth Service │                    │  User Service   │
      │    :8081      │                    │     :8082       │
      └───────────────┘                    └─────────────────┘
                             │
                             ▼
                    ┌─────────────────┐
                    │ Nutrition       │
                    │ Service :8083   │
                    └─────────────────┘
```

## 🚀 Features

* API Gateway using Spring Cloud Gateway
* Centralized request routing
* JWT authentication
* Public and protected endpoints
* JWT claim extraction
* User information propagation to downstream services
* Circuit Breaker with Resilience4j
* Fallback responses when services are unavailable
* Retry configuration for the authentication service
* CORS configuration
* Spring Boot Actuator
* Gradle build
* Java 21

## 🛠️ Technologies

| Technology           | Purpose                         |
| -------------------- | ------------------------------- |
| Java 21              | Programming language            |
| Spring Boot 3.2.4    | Application framework           |
| Spring Cloud Gateway | API Gateway and request routing |
| Spring Security      | Security configuration          |
| JWT                  | Authentication                  |
| JJWT                 | JWT parsing and validation      |
| Resilience4j         | Circuit Breaker                 |
| Spring Boot Actuator | Monitoring and management       |
| Gradle               | Build automation                |
| Lombok               | Boilerplate reduction           |

The project uses Java 21 and Spring Boot 3.2.4, with Spring Cloud Gateway, Spring Security, JJWT, Actuator and Resilience4j dependencies.

# 🔀 Request Routing

The Gateway exposes a unified API while internally forwarding requests to the appropriate microservice.

| Gateway Route       | Downstream Service |   Port |
| ------------------- | ------------------ | -----: |
| `/api/auth/**`      | Auth Service       | `8081` |
| `/api/users/**`     | User Service       | `8082` |
| `/api/nutrition/**` | Nutrition Service  | `8083` |

For example:

```text
Client
  │
  │ GET /api/nutrition/daily
  ▼
API Gateway :8080
  │
  │ Route
  ▼
Nutrition Service :8083
```

The routes are configured both through `application.properties` and programmatically through `GatewayConfig`.

# 🔐 Authentication

The Gateway validates JWT tokens before forwarding protected requests.

The authentication flow is:

```text
Client
   │
   │ Authorization: Bearer <JWT>
   ▼
API Gateway
   │
   ├── Is endpoint public?
   │       │
   │       ├── Yes ──► Forward request
   │       │
   │       └── No
   │
   ├── Extract JWT
   │
   ├── Validate signature
   │
   ├── Validate expiration
   │
   ├── Extract username
   │
   ├── Extract roles
   │
   └── Forward request
            │
            ▼
       Microservice
```

The `JwtAuthenticationFilter` is implemented as a Spring Cloud Gateway `GlobalFilter`. It checks the `Authorization` header, validates the token, extracts the username and roles, and adds them to downstream request headers.

The following endpoints are configured as public:

```text
/api/auth/login
/api/auth/register
/api/auth/refresh-token
/actuator/health
```

All other requests require authentication.

## JWT Headers

After successful authentication, the Gateway propagates user information to downstream services:

```http
X-User-Id: user@example.com
X-User-Roles: USER,ADMIN
```

This allows downstream microservices to consume authenticated user information without implementing the initial authentication flow themselves.

## JWT Configuration

JWT configuration is provided through environment variables:

```properties
spring.security.jwt.secret=${JWT_SECRET:your-very-secure-jwt-secret-key-for-production}
spring.security.jwt.expiration=86400000
```

For production environments, configure `JWT_SECRET` through a secure secret-management mechanism instead of relying on the default value.

# 🛡️ Circuit Breaker

The Gateway uses Resilience4j through Spring Cloud Circuit Breaker.

Each backend service has a Circuit Breaker and a fallback endpoint.

```text
Client
   │
   ▼
API Gateway
   │
   ▼
Circuit Breaker
   │
   ├── Service available
   │       │
   │       ▼
   │   Backend Service
   │
   └── Service unavailable
           │
           ▼
       Fallback
           │
           ▼
        HTTP 503
```

Configured services:

```text
authServiceCB
userServiceCB
nutritionServiceCB
```










