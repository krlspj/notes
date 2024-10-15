# Event-Driven Architecture in Go

This project demonstrates an Event-Driven Architecture implementation in Go.

## Folder Structure

```
event-driven-architecture/
├── cmd/
│   └── main.go
├── internal/
│   ├── domain/
│   │   ├── user.go
│   │   └── order.go
│   ├── events/
│   │   ├── event.go
│   │   ├── user_events.go
│   │   └── order_events.go
│   ├── publishers/
│   │   ├── kafka_publisher.go
│   │   └── rabbitmq_publisher.go
│   ├── subscribers/
│   │   ├── user_subscriber.go
│   │   └── order_subscriber.go
│   ├── handlers/
│   │   ├── user_handler.go
│   │   └── order_handler.go
│   ├── services/
│   │   ├── user_service.go
│   │   └── order_service.go
│   └── repositories/
│       ├── user_repository.go
│       └── order_repository.go
├── pkg/
│   ├── kafka/
│   │   └── kafka.go
│   ├── rabbitmq/
│   │   └── rabbitmq.go
│   └── database/
│       └── mysql.go
└── go.mod
```

## Component Explanation

- `cmd/`: Contains the main application entry point.
- `internal/`: Contains the core application code.
  - `domain/`: Defines core business entities.
  - `events/`: Defines event structures and types.
  - `publishers/`: Implements event publishing logic for different message brokers.
  - `subscribers/`: Implements event subscription and consumption logic.
  - `handlers/`: Contains event handlers that process specific events.
  - `services/`: Implements business logic and coordinates between events, handlers, and repositories.
  - `repositories/`: Handles data persistence and retrieval.
- `pkg/`: Contains shared packages and utilities.
  - `kafka/` and `rabbitmq/`: Provide clients for respective message brokers.
  - `database/`: Provides database connection and utilities.

## Event Flow

1. An action triggers an event (e.g., user registration, order placement).
2. The relevant service creates an event and sends it to a publisher.
3. The publisher sends the event to a message broker (Kafka or RabbitMQ).
4. Subscribers listen for specific events from the message broker.
5. When an event is received, it's passed to the appropriate handler.
6. The handler processes the event, potentially updating the database or triggering other actions.

## Key Concepts

- **Loose Coupling**: Components communicate through events, reducing direct dependencies.
- **Scalability**: Easy to add new publishers, subscribers, and handlers as the system grows.
- **Flexibility**: Can easily switch between different message brokers or add new ones.
- **Resilience**: If a component fails, events can be reprocessed when it recovers.

This structure allows for a highly decoupled, scalable system where components can evolve independently as long as they adhere to the agreed-upon event structures.
