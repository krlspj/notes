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

Example 2 hexagonal
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
