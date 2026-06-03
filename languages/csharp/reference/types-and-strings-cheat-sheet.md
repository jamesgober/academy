# Types And Strings Cheat Sheet

[C# Reference](./README.md)

---

## Related Lessons

- [Types, Variables, And Strings](../course/02-core-language-basics/01-types-variables-and-strings.md)
- [Methods, Parameters, And Returns](../course/02-core-language-basics/02-methods-parameters-and-returns.md)
- [Conditionals, Switch, And Pattern Matching](../course/02-core-language-basics/03-conditionals-switch-and-pattern-matching.md)
- [Loops And Iteration Patterns](../course/02-core-language-basics/04-loops-and-iteration-patterns.md)

---

## Numeric Types

| Type | Use | Notice |
|---|---|---|
| `int` | normal whole numbers | 32-bit signed |
| `long` | larger whole numbers | 64-bit signed |
| `float` | smaller floating-point | less common in app code |
| `double` | general floating-point | not ideal for money |
| `decimal` | money/high precision decimal | use `m` suffix |

Examples:

```csharp
int count = 3;
long fileSize = 9000000000L;
double ratio = 0.75;
decimal price = 19.99m;
```

---

## Other Common Types

```csharp
bool isActive = true;
char initial = 'A';
string name = "Ada";
DateTimeOffset now = DateTimeOffset.UtcNow;
Guid id = Guid.NewGuid();
```

---

## `var`

```csharp
var count = 3;       // int
var name = "Ada";    // string
var price = 19.99m;  // decimal
```

Notice:

```text
var is still strongly typed. It is not dynamic.
```

---

## Nullable Values

```csharp
int? maybeCount = null;
string? maybeName = Console.ReadLine();
```

Check:

```csharp
if (maybeName is not null)
{
    Console.WriteLine(maybeName);
}
```

Fallback:

```csharp
string name = maybeName ?? "Anonymous";
```

---

## String Essentials

```csharp
string name = "Mia";
string message = $"Hello {name}";

bool hasPrefix = name.StartsWith("M");
bool hasText = !string.IsNullOrWhiteSpace(name);
string trimmed = name.Trim();
string upper = name.ToUpperInvariant();
```

Prefer interpolation:

```csharp
$"{quantity} x {unitPrice:C} = {total:C}"
```

---

## Parsing User Input

```csharp
if (!int.TryParse(Console.ReadLine(), out int quantity))
{
    Console.WriteLine("Enter a whole number.");
}
```

```csharp
if (!decimal.TryParse(Console.ReadLine(), out decimal price))
{
    Console.WriteLine("Enter a valid price.");
}
```

Use `TryParse` for normal user input.

---

## Risk Notices

- Use `decimal`, not `double`, for money.
- `Console.ReadLine()` can return `null`.
- `Parse` throws on bad input; `TryParse` returns `false`.
- Strings are immutable; methods return new strings.
- `var` can hurt readability when the right side is unclear.

---

[C# Reference](./README.md)
