<h1 align="center">
    <img width="99" alt="C# logo" src="../../../../_assets/logos/cs.svg">
    <br>
    <b>C#</b>
</h1>

[Home](../../../../README.md) / [C#](../../README.md) / [Chapter 04](./README.md)

---

# Exception Handling And Failure Design

> Error handling is part of design. Good code does not merely hope everything
> works; it decides which failures are expected, where to report them, and which
> bugs should stop the program loudly.

**You will learn:**
- What exceptions are for
- How `try`, `catch`, and `finally` work
- When to throw
- When to return `false` or `null`
- How to preserve stack traces
- Where exceptions should be caught
- How to design user-friendly failure paths

**Before this page, you should know:** [Generic Collections In Practice](./01-generic-collections-in-practice.md)

---

## Exceptions In Plain Language

An exception means:

```text
Normal execution cannot continue from here.
```

Examples:

- required argument is invalid
- file does not exist
- JSON is malformed
- network request fails
- object is in the wrong state for the requested operation

Exceptions are not evil. Silent failure is worse.

---

## Basic `try` / `catch`

```csharp
try
{
    decimal amount = decimal.Parse(input);
    Console.WriteLine($"Amount: {amount}");
}
catch (FormatException)
{
    Console.WriteLine("Please enter a valid number.");
}
```

The `try` block contains code that may fail.

The `catch` block handles a specific failure.

---

## Catch Specific Exceptions First

```csharp
try
{
    string text = File.ReadAllText("orders.json");
    Console.WriteLine(text);
}
catch (FileNotFoundException)
{
    Console.WriteLine("No saved orders yet.");
}
catch (UnauthorizedAccessException)
{
    Console.WriteLine("You do not have permission to read that file.");
}
catch (IOException ex)
{
    Console.WriteLine($"File problem: {ex.Message}");
}
```

Specific catches let you give useful messages.

Avoid starting with:

```csharp
catch (Exception)
```

because it catches everything, including bugs you may not understand yet.

---

## Throwing Exceptions

Throw when a method cannot honestly do what it promised.

```csharp
public sealed class OrderItem
{
    public OrderItem(string sku, int quantity)
    {
        if (string.IsNullOrWhiteSpace(sku))
        {
            throw new ArgumentException("SKU is required.", nameof(sku));
        }

        if (quantity <= 0)
        {
            throw new ArgumentOutOfRangeException(
                nameof(quantity),
                "Quantity must be positive."
            );
        }

        Sku = sku;
        Quantity = quantity;
    }

    public string Sku { get; }
    public int Quantity { get; }
}
```

Use standard exception types first:

| Situation | Exception |
|---|---|
| bad argument value | `ArgumentException` |
| number outside allowed range | `ArgumentOutOfRangeException` |
| null argument when not allowed | `ArgumentNullException` |
| object state does not allow operation | `InvalidOperationException` |
| feature/path intentionally unsupported | `NotSupportedException` |

---

## Return Values For Expected Outcomes

Not every failure should be an exception.

If missing data is normal, return `null`:

```csharp
public Product? FindBySku(string sku)
{
    return _productsBySku.TryGetValue(sku, out Product? product)
        ? product
        : null;
}
```

If an operation may naturally fail, return `bool`:

```csharp
public bool TryAdd(Product product)
{
    return _productsBySku.TryAdd(product.Sku, product);
}
```

Rule:

```text
Use exceptions for broken promises.
Use return values for expected alternate outcomes.
```

---

## `finally`

`finally` runs whether the `try` succeeds or fails.

```csharp
FileStream? stream = null;

try
{
    stream = File.OpenRead("orders.json");
}
finally
{
    stream?.Dispose();
}
```

In modern C#, prefer `using` for disposable resources:

```csharp
using FileStream stream = File.OpenRead("orders.json");
```

But understanding `finally` helps you read older code.

---

## Preserve Stack Trace

Good:

```csharp
catch (Exception)
{
    throw;
}
```

Bad:

```csharp
catch (Exception ex)
{
    throw ex;
}
```

`throw;` preserves the original stack trace.

`throw ex;` resets it and makes debugging harder.

---

## Where To Catch Exceptions

Catch exceptions at boundaries:

- console app command loop
- web API controller or middleware
- background job runner
- file import/export workflow
- test expecting a failure

Avoid catching deep inside domain logic unless you can actually recover or add
useful context.

Boundary example:

```csharp
try
{
    OrderItem item = new OrderItem(sku, quantity);
    order.AddItem(item);
}
catch (ArgumentException ex)
{
    Console.WriteLine($"Could not add item: {ex.Message}");
}
```

---

## Real Example: Parse Command

```csharp
public static bool TryReadPositiveInt(string text, out int value)
{
    value = 0;

    if (!int.TryParse(text, out int parsed))
    {
        return false;
    }

    if (parsed <= 0)
    {
        return false;
    }

    value = parsed;
    return true;
}
```

Use:

```csharp
if (!TryReadPositiveInt(input, out int quantity))
{
    Console.WriteLine("Quantity must be a positive whole number.");
    return;
}
```

This is better than throwing for normal bad user input.

---

## Common Mistakes

### Mistake 1: Swallowing Exceptions

```csharp
catch
{
}
```

Now the program hides the failure and continues with mystery state.

### Mistake 2: Catching Too Broad Too Early

```csharp
catch (Exception ex)
{
    Console.WriteLine(ex.Message);
}
```

This may hide programming bugs. Catch specific exceptions when you know how to
handle them.

### Mistake 3: Exceptions For Simple User Input

Use `TryParse` for user-entered values instead of making exceptions part of the
normal path.

---

## Chapter Checkpoint

You should now be able to answer:

- What does an exception mean?
- When should you throw?
- When should you return `false` or `null`?
- Why catch specific exceptions first?
- What does `finally` do?
- Why is `throw;` different from `throw ex;`?
- Where should app-level exceptions usually be caught?

---

## Recap

- Exceptions represent broken promises or unrecoverable local failures.
- Return values are better for expected alternate outcomes.
- Catch specific exceptions close to application boundaries.
- Do not swallow exceptions silently.
- Use `throw;` to preserve stack traces.
- Use `TryParse` for normal user input failure.

## Try It Yourself

Write a command parser that reads quantity input:

- blank input fails gracefully
- non-number input fails gracefully
- zero or negative input fails gracefully
- positive input succeeds

---

[**Next ->** LINQ Query Patterns](./03-linq-query-patterns.md)  
[**<- Previous** Generic Collections In Practice](./01-generic-collections-in-practice.md)
