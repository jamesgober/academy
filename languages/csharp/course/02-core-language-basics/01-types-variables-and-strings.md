<h1 align="center">
    <img width="99" alt="C# logo" src="../../../../_assets/logos/cs.svg">
    <br>
    <b>C#</b>
</h1>

[Home](../../../../README.md) / [C#](../../README.md) / [Chapter 02](./README.md)

---

# Types, Variables, And Strings

> Reliable C# starts with choosing the right type, giving values clear names,
> and handling text/input without pretending users always type perfect data.

**You will learn:**
- What variables are
- Common C# built-in types
- When to use `int`, `long`, `double`, and `decimal`
- How `var` works
- How nullable values work
- How strings and interpolation work
- How to parse beginner console input safely

**Before this page, you should know:** [Project Commands](../01-getting-started/03-project-commands-new-build-run.md)

---

## Variables

```csharp
int count = 3;
decimal price = 19.99m;
bool isActive = true;
string name = "Ada";
```

Read:

```text
count is an int.
price is a decimal.
isActive is a bool.
name is a string.
```

C# local variables must be definitely assigned before use. That protects you
from many uninitialized-value mistakes.

---

## Common Type Map

| Type | Use | Example |
|---|---|---|
| `int` | normal whole numbers | `42` |
| `long` | larger whole numbers | `9000000000L` |
| `double` | scientific/general floating-point math | `3.14` |
| `decimal` | money/high precision decimal values | `19.99m` |
| `bool` | true/false | `true` |
| `char` | one character | `'A'` |
| `string` | text | `"hello"` |
| `DateTimeOffset` | date/time with offset | `DateTimeOffset.UtcNow` |
| `Guid` | unique identifier | `Guid.NewGuid()` |

Money rule:

```text
Use decimal for money-like values.
```

`double` can have tiny precision drift because it is binary floating-point.

---

## `var`

`var` asks C# to infer the type from the right side.

```csharp
var count = 3;              // int
var title = "Order";        // string
var price = 19.99m;         // decimal
```

Use `var` when the type is obvious.

Good:

```csharp
var total = price * quantity;
```

Less helpful:

```csharp
var result = Load();
```

if a beginner cannot tell what `Load()` returns.

---

## Strings

```csharp
string first = "Ada";
string last = "Lovelace";
string full = $"{first} {last}";
```

String interpolation:

```csharp
Console.WriteLine($"{full} wrote software history.");
```

Common string operations:

```csharp
string text = "  Hello C#  ";

Console.WriteLine(text.Length);
Console.WriteLine(text.Trim());
Console.WriteLine(text.ToUpperInvariant());
Console.WriteLine(text.Contains("C#"));
Console.WriteLine(text.StartsWith("  He"));
```

Strings are immutable:

```text
String methods return new strings. They do not change the original string.
```

---

## Nullable Values

Some variables can intentionally hold no value.

```csharp
int? maybeCount = null;
```

Check:

```csharp
if (maybeCount.HasValue)
{
    Console.WriteLine(maybeCount.Value);
}
```

Reference types can be nullable too:

```csharp
string? input = Console.ReadLine();
```

The `?` tells readers and the compiler:

```text
This may be null.
```

---

## Null-Coalescing

Use `??` to provide a fallback.

```csharp
string? input = Console.ReadLine();
string name = input ?? "Anonymous";
```

Read:

```text
If input is not null, use input.
Otherwise use "Anonymous".
```

---

## Parsing Input

Risky:

```csharp
int quantity = int.Parse(Console.ReadLine() ?? "");
```

This throws if input is not a valid number.

Better for user input:

```csharp
Console.Write("Quantity: ");
string? input = Console.ReadLine();

if (!int.TryParse(input, out int quantity))
{
    Console.WriteLine("Please enter a whole number.");
    return;
}

if (quantity <= 0)
{
    Console.WriteLine("Quantity must be positive.");
    return;
}
```

`TryParse` returns `true` or `false` instead of throwing for normal bad input.

---

## Real Example: Receipt Line

```csharp
Console.Write("Item name: ");
string itemName = Console.ReadLine() ?? "";

if (string.IsNullOrWhiteSpace(itemName))
{
    Console.WriteLine("Item name is required.");
    return;
}

Console.Write("Unit price: ");
if (!decimal.TryParse(Console.ReadLine(), out decimal unitPrice))
{
    Console.WriteLine("Unit price must be a number.");
    return;
}

if (unitPrice < 0)
{
    Console.WriteLine("Unit price cannot be negative.");
    return;
}

Console.Write("Quantity: ");
if (!int.TryParse(Console.ReadLine(), out int quantity) || quantity <= 0)
{
    Console.WriteLine("Quantity must be a positive whole number.");
    return;
}

decimal total = unitPrice * quantity;

Console.WriteLine($"{itemName}: {quantity} x {unitPrice:C} = {total:C}");
```

This teaches:

- strings
- nullable input
- validation
- `decimal`
- interpolation
- friendly failure

---

## Common Mistakes

### Mistake 1: `double` For Money

Use `decimal` for money-like values.

### Mistake 2: Ignoring Null Input

`Console.ReadLine()` returns `string?`.

Always decide what null means.

### Mistake 3: `Parse` For Normal User Input

Use `TryParse` for user input.

Use `Parse` only when invalid data is truly exceptional or already impossible.

---

## Chapter Checkpoint

You should now be able to answer:

- What is a variable?
- Why use `decimal` for money?
- What does `var` do?
- What does `string?` mean?
- What does `??` do?
- Why is `TryParse` better for user input?
- What does string interpolation do?

---

## Recap

- Types describe what data can hold and do.
- `decimal` is best for money-like values.
- `var` is type inference, not dynamic typing.
- Strings are immutable.
- Nullable values must be handled intentionally.
- `TryParse` is the beginner-safe input pattern.

## Try It Yourself

Write a receipt-line program that asks for:

- item name
- unit price
- quantity

Then prints the total or a friendly validation message.

---

[**Next ->** Methods, Parameters, And Returns](./02-methods-parameters-and-returns.md)  
[**<- Previous** Chapter Start](./README.md)
