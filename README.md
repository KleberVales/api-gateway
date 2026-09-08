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


