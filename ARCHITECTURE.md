# SprintRoom - AI Context & Architecture Guidelines

## Overview
This is a .NET 8 Backend project following **Clean Architecture** and **Vertical Slicing** (in the Application layer). The goal is to keep the domain pure, use MediatR for CQRS, and separate technical concerns into the Infrastructure layer.

## Architecture Layers

### 1. Domain (`SprintRoom.Domain`)
- **Purity**: Zero external dependencies (No Entity Framework, no ASP.NET, no third-party libraries except standard .NET libraries).
- **Contents**: Entities, Value Objects, Enums, Domain Exceptions.
- **Responsibility**: Contains the core business rules and logic.

### 2. Application (`SprintRoom.Application`)
- **Pattern**: CQRS (Command Query Responsibility Segregation) using MediatR.
- **Organization**: Vertical Slicing by Feature (e.g., `Features/Rooms`, `Features/Sprints`). Each feature contains its own Commands, Queries, and DTOs.
- **Contents**: 
  - `Abstractions/`: Interfaces that Infrastructure will implement (e.g., `IRoomRepository`, `IJwtProvider`).
  - `Behaviours/`: MediatR Pipeline Behaviors (e.g., Validation, Logging).
  - `Exceptions/`: Use case specific exceptions (e.g., `ValidationException`).
- **Dependencies**: Depends ONLY on `SprintRoom.Domain`.

### 3. Infrastructure (`SprintRoom.Infrastructure`)
- **Responsibility**: Implementations of abstractions defined in the Application layer.
- **Contents**:
  - `Persistence/`: Entity Framework Core DbContext, Migrations, Repositories, and Configurations (Fluent API).
  - `Authentication/`: JWT Bearer setup, Claims generation.
- **Dependencies**: Depends on `SprintRoom.Application`. 

### 4. Presentation (`SprintRoom.Api`)
- **Responsibility**: Expose the Application logic via RESTful APIs.
- **Contents**: Controllers (thin wrappers around MediatR), Middlewares (Global Exception Handling), Dependency Injection registration via Extensions.
- **Dependencies**: Depends on `SprintRoom.Application` and `SprintRoom.Infrastructure`.

### 5. Tests
- `SprintRoom.Domain.UnitTests`: Isolated tests for pure domain logic.
- `SprintRoom.Application.UnitTests`: Use case tests using Mocks (e.g., Moq) for repositories.
- `SprintRoom.Api.IntegrationTests`: End-to-end testing of controllers using `WebApplicationFactory` and an in-memory/test database.

## Rules & Best Practices for AI Agents
1. **Never leak Infrastructure to Domain**: Entity Framework attributes like `[Table]` or `[Key]` must NOT be used in the Domain layer. Use Fluent API in Infrastructure (`Configurations` folder).
2. **Use CQRS**: Every endpoint should map to a Command (if modifying data) or a Query (if reading data).
3. **Thin Controllers**: Controllers should only receive the request, send it to MediatR, and return the result. No business logic in controllers.
4. **Validation**: Use FluentValidation in the Application layer, executed automatically via a MediatR Pipeline Behavior.
5. **No Boilerplate**: Do not generate default template code (like `WeatherForecast`). Always focus on actual domain models like `Rooms`, `Sprints`, `Users`.
6. **Async by Default**: All repository methods, MediatR handlers, and controller endpoints should be asynchronous (`Task`, `async/await`).

*Note for AI models*: Before generating code, confirm you are placing it in the correct layer according to this document.

