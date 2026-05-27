<h1 align="center">
    <img width="99" alt="C# logo" src="../../../../_assets/logos/cs.svg">
    <br>
    <b>C#</b>
</h1>

<!-- ===== HEAD NAV ===== -->
<div align="center">

[Home](../../../../README.md) · [C#](../../README.md) · [Chapter 04](./README.md)

</div>

---

# Exception Handling and Failure Design

> Error handling is part of design, not cleanup work after coding.

**You will learn:**
- How to use `try/catch/finally`
- When to throw exceptions
- How to preserve context for debugging

**Before this page, you should know:** methods and return values.

---

## Handling failures

```csharp
try
{
    var amount = decimal.Parse(input);
    ProcessPayment(amount);
}
catch (FormatException ex)
{
    Console.WriteLine($"Invalid amount: {ex.Message}");
}
catch (Exception ex)
{
    Console.WriteLine($"Unexpected error: {ex.Message}");
    throw;
}
```

Guidance:
- catch specific exceptions first
- avoid swallowing exceptions silently
- rethrow when caller should handle or log globally

## Throwing exceptions

```csharp
if (amount < 0)
    throw new ArgumentOutOfRangeException(nameof(amount));
```

---

## Recap

- Catch expected failures, surface actionable messages.
- Throw argument exceptions for invalid method input.
- Preserve stack trace by rethrowing correctly.

## Try it yourself

Wrap one parsing operation in a `try/catch` and report a user-friendly error.

---

<!-- ===== FOOT NAV ===== -->
<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Generic Collections in Practice](./01-generic-collections-in-practice.md) | [Chapter](./README.md) · [Track](../../README.md) · [Home](../../../../README.md) | [LINQ Query Patterns →](./03-linq-query-patterns.md) |

</div>
