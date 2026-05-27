<h1 align="center">
    <img width="99" alt="C# logo" src="../../../../_assets/logos/cs.svg">
    <br>
    <b>C#</b>
</h1>

<!-- ===== HEAD NAV ===== -->
<div align="center">

[Home](../../../../README.md) · [C#](../../README.md) · [Chapter 05](./README.md)

</div>

---

# Unit Testing with xUnit

> Tests let you refactor with confidence and catch regressions early.

**You will learn:**
- How to create a test project
- Basic `Fact` and `Theory` usage
- How to run tests from terminal

**Before this page, you should know:** methods and class design.

---

## Setup

```bash
dotnet new xunit -n app.tests
dotnet test
```

## Example tests

```csharp
public class MathTests
{
    [Fact]
    public void Add_ReturnsExpectedValue()
    {
        Assert.Equal(5, 2 + 3);
    }

    [Theory]
    [InlineData(2, 2, 4)]
    [InlineData(1, 3, 4)]
    public void Add_Theory(int a, int b, int expected)
    {
        Assert.Equal(expected, a + b);
    }
}
```

## Testing workflow

- write a failing test
- implement/fix code
- rerun tests
- keep all tests green

---

## Recap

- Tests are executable requirements.
- `Fact` is single case; `Theory` handles multiple inputs.
- `dotnet test` should be part of your daily loop.

## Try it yourself

Write tests for one validation method with both success and failure inputs.

---

<!-- ===== FOOT NAV ===== -->
<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Async/Await and Task Basics](./01-async-await-and-task-basics.md) | [Chapter](./README.md) · [Track](../../README.md) · [Home](../../../../README.md) | [Debugging and Logging Workflow →](./03-debugging-and-logging-workflow.md) |

</div>
