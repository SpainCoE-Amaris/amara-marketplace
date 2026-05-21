---
name: cqrs-architecture
description: "CQRS expert skill for implementing complete CQRS architecture in .NET, from API controller layer to application, domain, and infrastructure layers."
categories:
  - cqrs
  - dotnet
  - architecture
tags:
  - CQRS
  - Mediator
  - CleanArchitecture
  - Controller
  - Application
  - Domain
  - Infrastructure
  - UnitTesting
  - IntegrationTesting
---

# CQRS Architecture Skill

Use this skill when implementing, refactoring, or extending CQRS-based features in a .NET solution with full end-to-end coverage.

## Scope

This skill covers the complete CQRS implementation stack:

- **API layer**: ASP.NET Core Controller / Minimal API endpoints
- **Application layer**: Commands, Queries, Handlers, DTOs, use cases
- **Domain layer**: Entities, Value Objects, Domain Events, invariants
- **Infrastructure layer**: EF Core repositories, DbContext, persistence, messaging
- **Cross-cutting concerns**: Validation, authorization, logging, error handling, auditing
- **Automation tests**: Unit tests for handlers/controllers and integration tests for persistence/API behavior

## Implementation Approach

1. **Requirements analysis** - Receive requirements and API surface (route, method, request/response contract).
2. **Application layer design** - Define use cases as clean application requests: `IRequest<TResponse>` for commands and queries.
3. **Request handlers** - Implement `IRequestHandler<TRequest,TResponse>` and keep controllers thin.
4. **Domain model** - Design in `Domain`: aggregates, invariants, and business rules.
5. **Application services** - Add services/ports in `Application` (repositories, unit-of-work interfaces, service abstractions).
6. **Infrastructure** - Add repository implementations in `Infrastructure` (EF Core, Dapper, etc.) and `DbContext` mapping.
7. **Validation & behaviors** - Add validation with `FluentValidation` and pipeline behaviors.
8. **Dependency injection** - Wire DI in `Program.cs` or `Startup.cs`.
9. **Testing** - Add xUnit/NUnit/MSTest tests for handlers, controllers, repositories (InMemory/SQLite), and integration flows.
10. **Architecture principles** - Keep SOLID/DDD/clean architecture principles throughout.

## Key Characteristics

- Keep business logic in application/domain layers, not controllers
- Use constructor injection for dependencies
- Use `async`/`await` for all I/O operations
- Use explicit, strongly-typed DTOs; avoid magic strings
- Prefer immutable domain objects and side-effect free methods where possible
- Apply mediator pattern (MediatR) for command/query dispatch

## Trigger Phrases

- "Implement full CQRS"
- "Create controller + application layer"
- "Design commands, queries and handlers"
- "Refactor to clean architecture with CQRS"
- "Build CQRS feature"

## Related Skills

- `xunit-testing` - For comprehensive test coverage of CQRS components
- `dotnet-upgrade` - For modernizing CQRS implementations to latest .NET versions
