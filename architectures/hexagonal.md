# Hexagonal Architecture in Go

This project demonstrates a Hexagonal Architecture (Ports and Adapters) implementation in Go.

## Folder Structure

```
hexagonal-architecture/
├── cmd/
│   └── main.go
├── internal/
│   ├── core/
│   │   ├── domain/
│   │   │   ├── user.go
│   │   │   └── product.go
│   │   └── ports/
│   │       ├── user_port.go
│   │       └── product_port.go
│   ├── adapters/
│   │   ├── driven/
│   │   │   ├── mysql/
│   │   │   │   ├── user_repository.go
│   │   │   │   └── product_repository.go
│   │   │   └── redis/
│   │   │       └── cache.go
│   │   └── driving/
│   │       ├── http/
│   │       │   ├── handler/
│   │       │   │   ├── user_handler.go
│   │       │   │   └── product_handler.go
│   │       │   └── router.go
│   │       └── grpc/
│   │           └── server.go
│   └── application/
│       ├── user_service.go
│       └── product_service.go
├── pkg/
│   └── database/
│       ├── mysql.go
│       └── redis.go
└── go.mod
```

## Layer Explanation

- `cmd/`: Contains the main application entry point.
- `internal/`: Contains the core application code.
  - `core/`: Contains the domain models and port interfaces.
    - `domain/`: Defines core business entities.
    - `ports/`: Defines interfaces for primary and secondary ports.
  - `adapters/`: Contains the implementations of the ports.
    - `driven/`: Secondary/driven adapters (e.g., database, external services).
    - `driving/`: Primary/driving adapters (e.g., HTTP handlers, gRPC servers).
  - `application/`: Contains the application services that coordinate between ports.
- `pkg/`: Contains shared packages and utilities.

This structure separates the core business logic from external concerns and allows for easy swapping of adapters.

---

## Examples

```
/hexagonal-api
├── cmd/
│   └── api/
│       └── main.go
├── internal/
│   ├── common/
│   │   ├── infrastructure/
│   │   │   └── database.go
│   │   └── errors/
│   │       └── errors.go
│   ├── user/
│   │   ├── domain/
│   │   │   └── user.go
│   │   ├── application/
│   │   │   ├── ports/
│   │   │   │   ├── repositories.go
│   │   │   │   └── services.go
│   │   │   └── services/
│   │   │       └── user_service.go
│   │   ├── infrastructure/
│   │   │   └── repositories/
│   │   │       └── user_repository.go
│   │   └── interfaces/
│   │       └── http/
│   │           └── handlers/
│   │               └── user_handler.go
│   ├── product/
│   │   ├── domain/
│   │   │   └── product.go
│   │   ├── application/
│   │   │   ├── ports/
│   │   │   │   ├── repositories.go
│   │   │   │   └── services.go
│   │   │   └── services/
│   │   │       └── product_service.go
│   │   ├── infrastructure/
│   │   │   └── repositories/
│   │   │       └── product_repository.go
│   │   └── interfaces/
│   │       └── http/
│   │           └── handlers/
│   │               └── product_handler.go
│   └── customer/
│       ├── domain/
│       │   └── customer.go
│       ├── application/
│       │   ├── ports/
│       │   │   ├── repositories.go
│       │   │   └── services.go
│       │   └── services/
│       │       └── customer_service.go
│       ├── infrastructure/
│       │   └── repositories/
│       │       └── customer_repository.go
│       └── interfaces/
│           └── http/
│               └── handlers/
│                   └── customer_handler.go
└── pkg/
    └── shared/
        └── valueobjects/
            └── money.go
```
---

```
/hexagonal-api
├── cmd/
│   └── api/
│       ├── main.go                        # Entry point for the API
│       ├── bootstrap/
│       │   └── bootstrap.go               # Initializes dependencies and injects them into the layers
│       ├── handler/
│       │   ├── conversation_handler.go    # HTTP handlers related to conversations
│       │   ├── message_handler.go         # HTTP handlers related to messages
│       │   └── model.go                   # Request/response models
│       ├── middleware/
│       │   └── middleware.go              # Middleware (e.g., logging, authentication)
│       └── router/
│           └── router.go                  # API routing
├── internal/
│   ├── common/
│   │   ├── infrastructure/
│   │   │   └── database.go                # Database connection and initialization
│   │   └── errors/
│   │       └── errors.go                  # Custom error handling
│   ├── conversation/
│   │   ├── domain/
│   │   │   ├── bot_response.go            # Domain logic for bot responses
│   │   │   ├── conversation.go            # Main Conversation entity
│   │   │   ├── message_origin.go          # Message origins definitions
│   │   ├── application/
│   │   │   ├── ports/                     # Ports for repositories and services
│   │   │   │   ├── repositories.go        # Conversation repository interface
│   │   │   │   └── services.go            # Conversation service interface
│   │   │   └── services/
│   │   │       └── conversation_service.go # Business logic for conversations
│   │   ├── infrastructure/
│   │   │   └── repositories/
│   │   │       └── conversation_repository.go  # DB conversation repository implementation
│   ├── customer/
│   │   ├── domain/
│   │   │   └── customer.go                # Main Customer entity
│   │   ├── application/
│   │   │   ├── ports/                     # Ports for repositories and services
│   │   │   │   ├── repositories.go        # Customer repository interface
│   │   │   │   └── services.go            # Customer service interface
│   │   │   └── services/
│   │   │       └── customer_service.go     # Business logic for customers
│   │   ├── infrastructure/
│   │   │   └── repositories/
│   │   │       └── customer_repository.go  # DB customer repository implementation
│   ├── product/
│   │   ├── domain/
│   │   │   └── product.go                 # Main Product entity
│   │   ├── application/
│   │   │   ├── ports/                     # Ports for repositories and services
│   │   │   │   ├── repositories.go        # Product repository interface
│   │   │   │   └── services.go            # Product service interface
│   │   │   └── services/
│   │   │       └── product_service.go      # Business logic for products
│   │   ├── infrastructure/
│   │   │   └── repositories/
│   │   │       └── product_repository.go   # DB product repository implementation
└── testhelper/
    └── test_helper.go                     # Testing utilities
```
