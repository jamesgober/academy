# C# Conditionals And Loops Patterns

[C# Reference](./README.md) / [C#](../README.md)

Use this page when choosing between `if`, ternary, switch expressions, pattern
matching, and loop types. For the guided lesson, see [Conditionals, Switch, and Pattern Matching](../course/02-core-language-basics/03-conditionals-switch-and-pattern-matching.md).

## `if` / `else`

```csharp
if (score >= 60)
{
    Console.WriteLine("Pass");
}
else
{
    Console.WriteLine("Needs practice");
}
```

Use for a clear two-path decision.

## Guard Clause

```csharp
if (user is null)
{
    return;
}
```

Guard clauses handle invalid or special cases early so the normal path stays
less nested.

## Ternary

```csharp
string state = isReady ? "Ready" : "Pending";
```

Use ternary for short value selection. Avoid nested ternaries in beginner and
production code unless the expression is truly obvious.

## Switch Expression

```csharp
string size = total switch
{
    < 50m => "Small",
    < 200m => "Medium",
    _ => "Large"
};
```

Switch expressions are good when you are choosing a value.

The `_` arm means default.

## Switch Statement

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

Switch statements are useful when each case performs actions.

## Pattern Matching

```csharp
string Describe(object? value) => value switch
{
    null => "Null",
    int number when number < 0 => "Negative int",
    int => "Int",
    string text => $"String({text.Length})",
    _ => "Other"
};
```

Patterns let decisions consider type, value, and extra conditions.

## Loop Choices

Use `for` when you need an index:

```csharp
for (int index = 0; index < totals.Length; index++)
{
    Console.WriteLine(totals[index]);
}
```

Use `foreach` when you only need each item:

```csharp
foreach (decimal total in totals)
{
    Console.WriteLine(total);
}
```

Use `while` when repetition depends on a condition:

```csharp
while (isRunning)
{
    ReadNextCommand();
}
```

## Loop Control

```csharp
break;     // exit current loop
continue;  // skip to next iteration
```

Use these sparingly. If a loop has many exits, consider extracting a method.

## LINQ Alternative

Some loops can become readable LINQ:

```csharp
List<decimal> largeTotals = totals
    .Where(total => total >= 200m)
    .ToList();
```

Use LINQ when it improves readability. Use loops when step-by-step mutation or
debugging is clearer.

## Risk Notices

- Avoid nested ternary expressions.
- Order matters in switch expressions; earlier arms can capture later cases.
- Handle `null` explicitly when input may be nullable.
- Prefer `foreach` when the index is not needed.
- Avoid modifying a collection while iterating it with `foreach`.

---

[C# Reference](./README.md) / [C#](../README.md)
