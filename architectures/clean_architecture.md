# Clean Architecture in Go

This project demonstrates a Clean Architecture implementation in Go.

## Folder Structure

```
clean-architecture/
├── cmd/
│   └── main.go
├── internal/
│   ├── domain/
│   │   ├── user.go
│   │   └── product.go
│   ├── usecase/
│   │   ├── user_usecase.go
│   │   └── product_usecase.go
│   ├── repository/
│   │   ├── user_repository.go
│   │   └── product_repository.go
│   └── delivery/
│       └── http/
│           ├── handler/
│           │   ├── user_handler.go
│           │   └── product_handler.go
│           └── router.go
├── pkg/
│   └── database/
│       └── mysql.go
└── go.mod
```

## Layer Explanation

- `cmd/`: Contains the main application entry point.
- `internal/`: Contains the core application code.
  - `domain/`: Defines core business logic and entities.
  - `usecase/`: Implements use cases (application-specific business rules).
  - `repository/`: Defines interfaces for data access.
  - `delivery/`: Handles the delivery of data to external systems or UI.
- `pkg/`: Contains shared packages and utilities.

This structure separates concerns and follows the dependency rule of Clean Architecture.
