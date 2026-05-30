# Errors, Warnings, And Debugging Guide

[C# Reference](./README.md)

---

## Related Lessons

- [Reading Errors And Warnings](../course/01-getting-started/04-reading-errors-and-warnings.md)
- [Exception Handling And Failure Design](../course/04-collections-exceptions-and-data/02-exception-handling-and-failure-design.md)
- [Unit Testing With xUnit](../course/05-async-testing-and-capstone/02-unit-testing-with-xunit.md)
- [Debugging And Logging Workflow](../course/05-async-testing-and-capstone/03-debugging-and-logging-workflow.md)

---

## Diagnostic Format

```text
Program.cs(10,13): error CS0103: The name 'x' does not exist in the current context
```

Read:

| Part | Meaning |
|---|---|
| `Program.cs` | file |
| `10` | line |
| `13` | column |
| `error` | severity |
| `CS0103` | diagnostic ID |
| message | what failed |

---

## Common Compiler Diagnostics

| ID | Common meaning | Typical fix |
|---|---|---|
| `CS0103` | name does not exist | fix typo, scope, or declaration |
| `CS1002` | missing `;` | add semicolon or fix syntax before it |
| `CS0246` | type/namespace not found | add using, reference, or package |
| `CS1503` | argument type mismatch | pass correct type or convert intentionally |
| `CS7036` | required argument missing | pass required constructor/method argument |
| `CS8618` | non-nullable property not initialized | initialize in constructor or mark nullable |
| `CS8602` | possible null dereference | null-check before use |

---

## Nullability Warnings

Example:

```csharp
Product? product = catalog.FindBySku("KB-100");
Console.WriteLine(product.Name); // warning
```

Fix:

```csharp
if (product is not null)
{
    Console.WriteLine(product.Name);
}
```

Notice:

```text
Nullability warnings often predict real runtime crashes.
```

---

## Stack Trace Reading

```text
System.ArgumentOutOfRangeException: Quantity must be positive.
   at OrderTracker.Core.OrderItem..ctor(...)
   at OrderTracker.App.Program.Main()
```

Read:

```text
exception type
message
where it was thrown
how the program got there
```

---

## Debugging Workflow

1. Reproduce reliably.
2. Record expected versus actual behavior.
3. Read the first error or exception.
4. Shrink the input.
5. Set breakpoints near the boundary.
6. Inspect parameters and object state.
7. Fix the root cause.
8. Add a regression test.
9. Rerun `dotnet build` and `dotnet test`.

---

## Logging Checklist

Good logs include:

- operation name
- relevant id, file path, or command
- success/failure status
- exception details when appropriate
- no secrets

Structured logging pattern:

```csharp
logger.LogInformation(
    "Order {OrderId} saved to {Path}",
    orderId,
    path
);
```

---

## Bug Report Template

```md
## Expected

## Actual

## Steps To Reproduce

## Input

## Environment

## First Error Or Stack Trace
```

---

[C# Reference](./README.md)
