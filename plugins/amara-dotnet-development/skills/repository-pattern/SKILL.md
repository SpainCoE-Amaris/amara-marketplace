---
name: repository-pattern
description: "Repository Pattern expert skill for designing persistence abstractions and testable data access in .NET applications."
categories:
  - repository
  - dotnet
  - architecture
  - data-access
tags:
  - Repository Pattern
  - Persistence
  - Data Access
  - Abstraction
  - Testability
  - Clean Architecture
  - DDD
  - UnitOfWork
---

# Repository Pattern Skill

Use this skill when implementing, refactoring, or reviewing repository abstractions in .NET solutions to improve maintainability and testability.

## Scope

This skill covers:

- **Repository abstractions**: defining repositories for domain aggregates and data access boundaries
- **Persistence independence**: separating domain logic from infrastructure concerns
- **Testability**: enabling unit tests through repository interfaces and in-memory/stub implementations
- **Repository design**: generic repositories, aggregate-specific repositories, and read/write separation
- **Integration with DDD**: repository as a persistence gateway for aggregates
- **Unit of Work**: coordinating transactional changes across repositories where needed
- **Infrastructure implementation**: EF Core, Dapper, raw SQL, and other persistence providers behind repository contracts

## Implementation Approach

1. **Define repository interfaces** - Use clear, intent-focused interfaces in the domain or application layer.
2. **Keep contracts small** - Expose only persistence operations required by domain use cases.
3. **Model repositories around aggregates** - Use aggregate roots as repository boundaries.
4. **Avoid leaking persistence details** - Keep IQueryable, DbContext, and ORM-specific types out of repository contracts.
5. **Implement repository classes** - Add infrastructure implementations in a separate layer.
6. **Use Unit of Work where needed** - Coordinate save operations across multiple repositories for transactional consistency.
7. **Register dependencies** - Wire repository implementations into DI with scoped lifetimes.
8. **Test repository usage** - Mock repository interfaces in domain/application tests and use integration tests for persistence behavior.
9. **Support query projections** - Use read models or query-specific methods when needed for performance.
10. **Evolve as needs change** - Refactor repository abstractions when domain requirements or persistence strategies evolve.

## Key Characteristics

- Favor explicit repository methods over generic CRUD surfaces when business behavior is involved
- Keep repositories focused on persistence, not business rules
- Use repositories as an anti-corruption boundary for external data sources
- Prefer interfaces to enable mocking and inversion of control
- Keep repository logic thin; delegate complex transformation to dedicated mapping or query services

## Trigger Phrases

- "Implement the repository pattern"
- "Create data access repositories"
- "Refactor persistence into repositories"
- "Design repository interfaces for aggregates"
- "Add repository and unit of work abstractions"

## Related Skills

- `ddd` - For domain model patterns and aggregate boundaries that inform repository design
- `cqrs-architecture` - For repository usage within CQRS command/query flows
- `xunit-testing` - For testing repository-driven application logic and persistence contracts
