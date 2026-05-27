# Types and Strings Cheat Sheet

## Numeric types

- `int` (32-bit signed)
- `long` (64-bit signed)
- `float` (single precision)
- `double` (double precision)
- `decimal` (high-precision decimal, best for money)

## Other common types

- `bool`
- `char`
- `string`
- `DateTime`
- `Guid`

## Nullable value types

```csharp
int? maybeCount = null;
```

## String essentials

```csharp
string name = "Mia";
string msg = $"Hello {name}";
bool hasPrefix = name.StartsWith("M");
```

Prefer interpolation over concatenation for readability.

---

[← C# Reference](./README.md)
