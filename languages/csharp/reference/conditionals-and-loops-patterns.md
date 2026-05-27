# Conditionals and Loops Patterns

## If and else-if chain

```csharp
if (x > 10) { }
else if (x > 5) { }
else { }
```

## Ternary

```csharp
string state = isReady ? "Ready" : "Pending";
```

## Switch expression

```csharp
string size = n switch
{
    < 10 => "S",
    < 100 => "M",
    _ => "L"
};
```

## Loop choices

- `for`: index-based iteration
- `foreach`: element-based iteration
- `while`: condition-driven repetition

Use descriptive loop variable names when logic is non-trivial.

---

[← C# Reference](./README.md)
