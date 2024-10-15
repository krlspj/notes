# Onion Architecture in Go

This project demonstrates an Onion Architecture implementation in Go.

## Folder Structure

```
onion-architecture/
├── cmd/
│   └── main.go
├── internal/
│   ├── domain/
│   │   ├── entities/
│   │   │   ├── user.go
│   │   │   └── product.go
│   │   └── interfaces/
│   │       ├── repositories/
│   │       │   ├── user_repository.go
│   │       │   └── product_repository.go
│   │       └── services/
│   │           ├── user_service.go
│   │           └── product_service.go
│   ├── application/
│   │   ├── services/
│   │   │   ├── user_service_impl.go
│   │   │   └── product_service_impl.go
│   │   └── dtos/
│   │       ├── user_dto.go
│   │       └── product_dto.go
│   ├── infrastructure/
│   │   ├── persistence/
│   │   │   ├── mysql/
│   │   │   │   ├── user_repository.go
│   │   │   │   └── product_repository.go
│   │   │   └── redis/
│   │   │       └── cache.go
│   │   └── api/
│   │       ├── http/
│   │       │   ├── handlers/
│   │       │   │   ├── user_handler.go
│   │       │   │   └── product_handler.go
│   │       │   └── router.go
│   │       └── grpc/
│   │           └── server.go
│   └── config/
│       └── app_config.go
├── pkg/
│   └── database/
│       ├── mysql.go
│       └── redis.go
└── go.mod
```

## Layer Explanation

- `cmd/`: Contains the main application entry point.
- `internal/`: Contains the core application code.
  - `domain/`: The core of the application, containing business logic.
    - `entities/`: Defines core business entities.
    - `interfaces/`: Defines interfaces for repositories and services.
  - `application/`: Contains application-specific logic and use cases.
    - `services/`: Implements domain service interfaces.
    - `dtos/`: Data Transfer Objects for input/output.
  - `infrastructure/`: Implements interfaces defined in the domain layer.
    - `persistence/`: Data storage implementations.
    - `api/`: API implementations (HTTP, gRPC).
  - `config/`: Application configuration.
- `pkg/`: Contains shared packages and utilities.

The Onion Architecture emphasizes the separation of concerns by organizing code in concentric circles (layers). The inner layers define interfaces, and the outer layers implement these interfaces. This structure ensures that the core business logic (domain) remains independent of external concerns.
