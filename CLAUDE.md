# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

Run from the repo root (where `SprintRoom.sln` lives). Target framework is .NET 8.

```bash
dotnet build                                                     # build the whole solution
dotnet run --project src/SprintRoom.Api/SprintRoom.Api.csproj    # run the API (Swagger at /swagger, http://localhost:5116 by default)
dotnet test                                                      # run all test projects
dotnet test tests/SprintRoom.Domain.UnitTests                    # run a single test project
dotnet test --filter "FullyQualifiedName~SomeClass.SomeMethod"   # run a single test (xUnit)
```

## Current state

The solution is a fresh scaffold: `Program.cs` is still the ASP.NET template and Domain/Application/Infrastructure contain no code yet. The test projects contain no tests yet. The packages called for by the architecture (MediatR, FluentValidation, EF Core, JWT bearer, Moq) are NOT yet referenced; add them to the correct project when first needed.

## Architecture

Clean Architecture with vertical slicing in the Application layer. Full guidelines live in `ARCHITECTURE.md`; read it before adding code and confirm the target layer.

Dependency direction (enforced by project references):

```
Api -> Application, Infrastructure
Infrastructure -> Application
Application -> Domain
Domain -> (nothing; standard .NET only)
```

- **Domain**: entities, value objects, enums, domain exceptions. No EF attributes (`[Key]`, `[Table]`) or any third-party packages.
- **Application**: CQRS with MediatR, organized by feature (`Features/Rooms`, `Features/Sprints`, ...), each with its own Commands, Queries and DTOs. `Abstractions/` holds interfaces implemented by Infrastructure (e.g. `IRoomRepository`, `IJwtProvider`). Validation uses FluentValidation run by a MediatR pipeline behavior in `Behaviours/`.
- **Infrastructure**: `Persistence/` (EF Core DbContext, migrations, repositories, Fluent API configurations in `Configurations/`) and `Authentication/` (JWT).
- **Api**: thin controllers that only dispatch to MediatR, global exception middleware, DI registration via extension methods.

Key conventions:
- Every endpoint maps to a Command (writes) or a Query (reads); no business logic in controllers.
- Everything async (repositories, handlers, endpoints).
- Never generate template boilerplate (e.g. `WeatherForecast`); model real domain concepts (Rooms, Sprints, Users).

Tests: `Domain.UnitTests` (pure domain logic), `Application.UnitTests` (handlers with mocked repositories), `Api.IntegrationTests` (`WebApplicationFactory` with an in-memory/test database). All use xUnit.

## Repository rules (from RULES.md)

- **No emojis anywhere**: code, config, Markdown, commit messages, PRs. No exceptions.
- Code identifiers in English; comments and docs may be in Spanish.
- Never commit secrets, JWT keys, or production connection strings; use environment variables or ignored local files (e.g. `appsettings.Development.json`, `.env`).
- `src/SprintRoom.Api/appsettings.Development.json` is git-ignored; the versioned template is `appsettings.Development.example.json`. When adding new config keys, add them to the example with placeholder values only.
