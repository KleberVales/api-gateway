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
