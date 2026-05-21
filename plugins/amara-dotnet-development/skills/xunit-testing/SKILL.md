---
name: xunit-testing
description: "xUnit testing expert skill for .NET projects, focused on quality, edge coverage, and best practices."
categories:
  - unit-testing
  - dotnet
  - xunit
tags:
  - xUnit
  - UnitTests
  - CleanCode
  - TDD
  - Mocking
  - FluentAssertions
  - TestAutomation
---

# xUnit Testing Skill

Use this skill to generate, improve, or review unit and integration tests in .NET projects using xUnit framework.

## Scope

This skill provides comprehensive testing guidance for:

- **Unit tests** - Test individual components in isolation using mocks
- **Integration tests** - Test component interactions and persistence layers
- **Test naming** - Follow clear naming conventions (`MethodName_Scenario_ExpectedResult`)
- **Test organization** - Arrange/Act/Assert pattern
- **Mocking** - Use Moq/NSubstitute for dependencies
- **Assertions** - Leverage FluentAssertions for readable assertions
- **Test isolation** - Constructor injection patterns for testability
- **Coverage** - Identify gaps and improve code coverage

## Implementation Approach

1. **Dependency analysis** - Detect target class/method and identify dependencies
2. **Testability design** - Prefer constructor injection + interfaces for easier mocks
3. **Test structure** - Build each test with Arrange/Act/Assert pattern
4. **Mocking strategy** - Use mocked dependencies (Moq/NSubstitute) where side effects exist
5. **Test naming** - Apply `MethodName_Scenario_ExpectedResult` naming format
6. **Special setup** - Document paths when tests require fixtures, DB, or config
7. **Traits & filtering** - Tag tests with `Trait` for route, performance, or integration properties
8. **Test execution** - Suggest `dotnet test --filter ...` and coverage tools (Coverlet/ReportGenerator)

## Test Coverage Expectations

- Minimum 3 test cases per method:
  1. **Happy path** - Normal operation with valid inputs
  2. **Edge case** - Boundary conditions and special scenarios
  3. **Failure case** - Error handling and exception paths
- Use FluentAssertions for expressive assertions
- Document complex test scenarios with comments

## Trigger Phrases

- "Create xUnit tests for this class"
- "Refactor xunit test suite"
- "Add xunit unit tests"
- "Improve unit test coverage with xunit"
- "Write tests for this method"
- "Build integration tests"

## Key Guidelines

- Focus on behavior, not implementation details
- Keep tests independent and repeatable
- Use descriptive assertion messages
- Group related tests in test fixtures
- Use test data builders or factory patterns for complex objects
- Isolate external dependencies with mocks

## Related Skills

- `cqrs-architecture` - For comprehensive CQRS implementations that require thorough testing
- `dotnet-upgrade` - For modernizing test infrastructure to latest .NET versions
