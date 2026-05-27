---
name: ddd
description: "Domain-Driven Design (DDD) expert skill for modeling domain-centric .NET solutions and aligning code with business concepts."
categories:
  - ddd
  - dotnet
  - architecture
  - domain
tags:
  - Domain-Driven Design
  - DDD
  - Aggregates
  - Entities
  - Value Objects
  - Domain Events
  - Bounded Contexts
  - Ubiquitous Language
  - Clean Architecture
---

# Domain-Driven Design Skill

Use this skill when modeling, implementing, refactoring, or reviewing .NET solutions around core business domain concepts.

## Scope

This skill focuses on:

- **Domain modeling**: aggregates, entities, value objects, domain events, and domain services
- **Bounded contexts**: defining clear boundaries, context maps, and integration contracts
- **Ubiquitous language**: aligning code and design with business terminology
- **Strategic design**: core, supporting, and generic subdomains
- **Tactical patterns**: aggregates, factories, repositories, specifications, and domain services
- **Integration patterns**: anti-corruption layers, published language, and upstream/downstream context coordination
- **Clean architecture**: keeping domain logic isolated from application, infrastructure, and UI concerns
- **Testability**: domain-focused unit tests, behavior verification, and invariants

## Implementation Approach

1. **Understand the business domain** - Capture domain concepts, roles, workflows, constraints, and ubiquitous language from requirements.
2. **Define bounded contexts** - Separate distinct business domains into explicit contexts with clear boundaries.
3. **Model aggregates** - Identify aggregate roots, consistency boundaries, and transactional invariants.
4. **Define domain objects** - Implement entities, value objects, domain events, and domain services in the domain layer.
5. **Use repositories and factories** - Provide persistence and construction through well-defined repository and factory abstractions.
6. **Keep domain logic central** - Ensure business rules live in the domain model rather than application services or controllers.
7. **Apply integration anti-corruption** - Protect the core domain from external models with translation layers.
8. **Enforce invariants** - Validate rules within aggregates and use domain events to capture state changes.
9. **Add tests** - Cover domain behavior, invariants, factory logic, and integration scenarios with focused tests.
10. **Evolve the model** - Refactor domain concepts as the business grows, keeping code aligned with the latest domain understanding.

## Key Characteristics

- Prioritize **behavior and intent** over technical detail
- Keep domain objects **immutable** where possible
- Use explicit domain types instead of primitive strings or numbers
- Avoid anemic domain models by placing business rules inside aggregates and domain services
- Maintain **clean separation** between domain, application, and infrastructure
- Favor expressive, business-driven naming and patterns
- Use `DomainEvents` to represent important business changes and triggers

## Trigger Phrases

- "Design a DDD domain model"
- "Implement aggregates and domain events"
- "Refactor to domain-driven design"
- "Create bounded contexts and context mapping"
- "Build a domain-centric .NET architecture"

## Related Skills

- `cqrs-architecture` - For CQRS implementations that benefit from domain-centric modeling
- `xunit-testing` - For verifying domain invariants and behavior through tests
- `dotnet-upgrade` - For modernizing DDD solutions to current .NET versions
