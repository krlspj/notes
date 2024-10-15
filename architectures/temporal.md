# Temporal Architecture in Go

This project demonstrates a Temporal Architecture implementation in Go, showcasing how to structure a project using Temporal for workflow orchestration.

## Folder Structure

```
temporal-architecture/
├── cmd/
│   └── main.go
├── internal/
│   ├── domain/
│   │   ├── user.go
│   │   └── order.go
│   ├── workflows/
│   │   ├── user_workflow.go
│   │   └── order_workflow.go
│   ├── activities/
│   │   ├── user_activities.go
│   │   └── order_activities.go
│   ├── services/
│   │   ├── user_service.go
│   │   └── order_service.go
│   ├── repositories/
│   │   ├── user_repository.go
│   │   └── order_repository.go
│   └── api/
│       └── http/
│           ├── handlers/
│           │   ├── user_handler.go
│           │   └── order_handler.go
│           └── router.go
├── pkg/
│   ├── temporal/
│   │   ├── client.go
│   │   └── worker.go
│   └── database/
│       └── mysql.go
├── config/
│   └── config.go
└── go.mod
```

## Component Explanation

- `cmd/`: Contains the main application entry point.
- `internal/`: Contains the core application code.
  - `domain/`: Defines core business entities.
  - `workflows/`: Contains Temporal workflow definitions.
  - `activities/`: Contains Temporal activity implementations.
  - `services/`: Implements business logic and coordinates between workflows and activities.
  - `repositories/`: Handles data persistence and retrieval.
  - `api/`: Contains API handlers and routing logic.
- `pkg/`: Contains shared packages and utilities.
  - `temporal/`: Provides Temporal client and worker setup.
  - `database/`: Provides database connection and utilities.
- `config/`: Contains application configuration.

## Temporal Concepts

1. **Workflows**: Long-running, fault-tolerant business processes defined in `workflows/`. They orchestrate the execution of activities and handle complex logic, retries, and error handling.

2. **Activities**: Individual tasks or operations defined in `activities/`. They are the building blocks of workflows and typically interact with external systems or perform discrete units of work.

3. **Workers**: Processes that execute workflows and activities. They are set up in `pkg/temporal/worker.go`.

4. **Clients**: Used to start and manage workflows. The Temporal client is set up in `pkg/temporal/client.go`.

## Workflow Example

A typical workflow might look like this:

```go
// in workflows/order_workflow.go
func OrderWorkflow(ctx workflow.Context, orderID string) error {
    // Define workflow logic
    err := workflow.ExecuteActivity(ctx, activities.ValidateOrder, orderID).Get(ctx, nil)
    if err != nil {
        return err
    }

    err = workflow.ExecuteActivity(ctx, activities.ProcessPayment, orderID).Get(ctx, nil)
    if err != nil {
        return err
    }

    return workflow.ExecuteActivity(ctx, activities.ShipOrder, orderID).Get(ctx, nil)
}
```

## Key Benefits

- **Durability**: Temporal persists the state of workflows, allowing them to resume from where they left off in case of failures.
- **Scalability**: Easily scale by adding more workers as the load increases.
- **Visibility**: Temporal provides tools for monitoring and debugging complex workflows.
- **Versioning**: Supports versioning of workflows, allowing for safe updates to long-running processes.

This architecture is particularly useful for complex, long-running processes that need to be resilient to failures and scalable. It separates the orchestration logic (workflows) from the actual implementation of tasks (activities), providing a clear and maintainable structure for distributed applications.

## Example
```go
// in workflows/order_workflow.go
func OrderWorkflow(ctx workflow.Context, orderID string) error {
    logger := workflow.GetLogger(ctx)
    logger.Info("OrderWorkflow started", "orderID", orderID)

    var result string
    err := workflow.ExecuteActivity(ctx, activities.ProcessOrder, orderID).Get(ctx, &result)
    if err != nil {
        logger.Error("OrderWorkflow failed", "error", err)
        return err
    }

    logger.Info("OrderWorkflow completed", "result", result)
    return nil
}

// in activities/order_activities.go
func ProcessOrder(ctx context.Context, orderID string) (string, error) {
    // Implement order processing logic here
    return "Order " + orderID + " processed successfully", nil
}

```
