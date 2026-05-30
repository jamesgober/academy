# Conditionals and Loops Patterns

## If and else-if chain

```csharp
if (x > 10) { }
else if (x > 5) { }
else { }
```

Guard-clause pattern (if without else):

```csharp
if (user is null) return;
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

Switch statement variant:

```csharp
switch (command)
{
    case "start":
        Start();
        break;
    case "stop":
        Stop();
        break;
    default:
        Console.WriteLine("Unknown command");
        break;
}
```

Pattern matching variant:

```csharp
string Describe(object value) => value switch
{
    int n when n < 0 => "Negative int",
    int _ => "Int",
    string s => $"String({s.Length})",
    null => "Null",
    _ => "Other"
};
```

## Loop choices

- `for`: index-based iteration
- `foreach`: element-based iteration
- `while`: condition-driven repetition

Loop control:
- `break`: exit current loop
- `continue`: skip to next iteration

Use descriptive loop variable names when logic is non-trivial.

---

[C# Reference](./README.md)

