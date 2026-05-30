<h1 align="center">
    <img width="99" alt="C# logo" src="../../../../_assets/logos/cs.svg">
    <br>
    <b>C#</b>
</h1>

[Home](../../../../README.md) / [C#](../../README.md) / [Chapter 05](./README.md)

---

# Unit Testing With xUnit

> Tests are executable requirements. They let you prove behavior today and catch
> regressions when you improve code tomorrow.

**You will learn:**
- How to create an xUnit test project
- How to reference the code under test
- How `[Fact]` works
- How `[Theory]` and `[InlineData]` work
- How to test exceptions
- How to test async methods
- How to keep tests readable

**Before this page, you should know:** [Async/Await And Task Basics](./01-async-await-and-task-basics.md)

---

## Create A Test Project

```powershell
dotnet new xunit -n OrderTracker.Core.Tests
```

Add reference to the project being tested:

```powershell
dotnet add OrderTracker.Core.Tests/OrderTracker.Core.Tests.csproj reference OrderTracker.Core/OrderTracker.Core.csproj
```

Run:

```powershell
dotnet test
```

---

## `[Fact]`

A fact is one test case.

```csharp
public sealed class CalculatorTests
{
    [Fact]
    public void Add_ReturnsSum()
    {
        int result = 2 + 3;

        Assert.Equal(5, result);
    }
}
```

Name tests like behavior:

```text
MethodOrBehavior_Condition_ExpectedResult
```

Do not obsess over one naming style. Do make the behavior obvious.

---

## Arrange, Act, Assert

```csharp
[Fact]
public void Total_AddsLineTotals()
{
    // Arrange
    var order = new Order(1001);
    order.AddItem(new OrderItem("KB-100", 2, 10m));
    order.AddItem(new OrderItem("MS-200", 1, 5m));

    // Act
    decimal total = order.Total;

    // Assert
    Assert.Equal(25m, total);
}
```

The pattern:

```text
Arrange: create the world
Act: do the behavior
Assert: verify the result
```

---

## `[Theory]`

Use a theory for multiple input cases.

```csharp
public sealed class QuantityParserTests
{
    [Theory]
    [InlineData("1", true, 1)]
    [InlineData("10", true, 10)]
    [InlineData("0", false, 0)]
    [InlineData("-5", false, 0)]
    [InlineData("abc", false, 0)]
    public void TryReadPositiveInt_HandlesInputs(
        string input,
        bool expectedOk,
        int expectedValue
    )
    {
        bool ok = QuantityParser.TryReadPositiveInt(input, out int value);

        Assert.Equal(expectedOk, ok);
        Assert.Equal(expectedValue, value);
    }
}
```

Use theories when the test logic is the same but the data changes.

---

## Testing Exceptions

```csharp
[Fact]
public void Constructor_RejectsBlankSku()
{
    Assert.Throws<ArgumentException>(
        () => new OrderItem("", 1, 10m)
    );
}
```

For async:

```csharp
[Fact]
public async Task LoadAsync_InvalidJson_ThrowsJsonException()
{
    var store = new SettingsStore("bad-settings.json");

    await Assert.ThrowsAsync<JsonException>(
        () => store.LoadRequiredAsync()
    );
}
```

---

## Testing Async Success

```csharp
[Fact]
public async Task SaveAndLoadAsync_RoundTripsSettings()
{
    string path = Path.Combine(
        Path.GetTempPath(),
        $"{Guid.NewGuid()}.json"
    );

    var store = new SettingsStore(path);
    var original = new AppSettings("test", 5);

    await store.SaveAsync(original);
    AppSettings loaded = await store.LoadAsync();

    Assert.Equal(original, loaded);

    File.Delete(path);
}
```

Notice:

- use a temp file
- use `await`
- clean up the file
- assert the loaded value

---

## Testing With Fakes

If a service depends on an interface, tests can use a fake.

```csharp
public sealed class FakeNotifier : INotifier
{
    public List<string> Sent { get; } = new();

    public Task SendAsync(string message)
    {
        Sent.Add(message);
        return Task.CompletedTask;
    }
}
```

Test:

```csharp
[Fact]
public async Task ShipAsync_SendsNotification()
{
    var notifier = new FakeNotifier();
    var service = new OrderService(notifier);

    await service.ShipAsync(1001);

    Assert.Contains("Order 1001 shipped", notifier.Sent);
}
```

This tests behavior without sending real email.

---

## What To Test

Test:

- validation rules
- calculations
- state changes
- edge cases
- error paths
- serialization round-trips
- async behavior

Do not over-focus on:

- property getters with no logic
- framework behavior
- implementation details that can change without changing behavior

---

## Common Mistakes

### Mistake 1: Tests That Only Prove The Code Runs

```csharp
order.Total;
```

Assert something.

### Mistake 2: Too Much Setup

If setup is huge, your design may be too tightly coupled.

### Mistake 3: Blocking Async Tests

Avoid:

```csharp
service.SaveAsync().Wait();
```

Use:

```csharp
await service.SaveAsync();
```

---

## Chapter Checkpoint

You should now be able to answer:

- What does `[Fact]` mean?
- What does `[Theory]` mean?
- What are Arrange, Act, Assert?
- How do you test exceptions?
- How do you test async methods?
- Why are fakes useful?
- What behavior should you prioritize testing?

---

## Recap

- xUnit tests are executable requirements.
- `[Fact]` is one case.
- `[Theory]` runs the same test with multiple inputs.
- `Assert.Throws` verifies exceptions.
- Async tests should return `Task` and use `await`.
- Interfaces make fakes easy.

## Try It Yourself

Write tests for one validation method with:

- valid input
- blank input
- invalid number
- negative number
- async success path if applicable

---

[**Next ->** Debugging And Logging Workflow](./03-debugging-and-logging-workflow.md)  
[**<- Previous** Async/Await And Task Basics](./01-async-await-and-task-basics.md)
