---
name: test-xunit
description: "xUnit best-practices skill for .NET unit testing with built-in examples."
---

# xUnit Skill

This skill provides reference patterns, live snippet templates, and decision rules for xUnit test design.

## 1. Project Setup

- Test project: `[ProjectName].Tests`.
- NuGet: `Microsoft.NET.Test.Sdk`, `xunit`, `xunit.runner.visualstudio`, optional `Moq`, `NSubstitute`, `FluentAssertions`, `AutoFixture`.
- Use `.NET SDK` project format and `dotnet test`.

## 2. Test Structure

- No test class attributes required (unlike MSTest/NUnit).
- Use `[Fact]` for simple tests, `[Theory]` for parameterized tests.
- Use AAA: Arrange, Act, Assert.
- Naming: `MethodName_Scenario_ExpectedResult`.
- Use constructor for setup and `IDisposable.Dispose` for teardown.
- Share context with `IClassFixture<T>`/`ICollectionFixture<T>`.

## 3. Template Snippets

### Fact test

```csharp
public class CalculatorTests
{
    [Fact]
    public void Add_WhenCalledWithTwoNumbers_ReturnsSum()
    {
        // Arrange
        var calculator = new Calculator();

        // Act
        var result = calculator.Add(2, 3);

        // Assert
        result.Should().Be(5);
    }
}
```

### Theory test

```csharp
[Theory]
[InlineData(2, 3, 5)]
[InlineData(-1, 4, 3)]
public void Add_ReturnsExpectedSum(int a, int b, int expected)
{
    var calculator = new Calculator();
    var result = calculator.Add(a, b);
    result.Should().Be(expected);
}
```

### Async exception assertion

```csharp
[Fact]
public async Task Authenticate_InvalidCredentials_ThrowsUnauthorizedAccessException()
{
    var service = new AuthService(...);
    Func<Task> action = async () => await service.AuthenticateAsync("bad", "bad");
    await action.Should().ThrowAsync<UnauthorizedAccessException>().WithMessage("Invalid login");
}
```

## 4. Assertions

- `Assert.Equal` for value equality.
- `Assert.Same` for reference identity.
- `Assert.True`/`Assert.False` for conditions.
- `Assert.Throws<T>`/`Assert.ThrowsAsync<T>` for exceptions.
- Prefer `FluentAssertions` for readability.

## 5. Mocking and Isolation

- Use Moq or NSubstitute.
- Isolate unit under test via interfaces.
- Avoid external resources except integration tests.

## 6. CI/Reporting

- `dotnet test --logger "trx;LogFileName=testresults.trx" --collect "XPlat Code Coverage"`.
- Use Coverlet and ReportGenerator for coverage artifacts.
- Use `--filter Category=...` for targeted test runs.
