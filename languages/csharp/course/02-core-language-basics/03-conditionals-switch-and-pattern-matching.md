<h1 align="center">
    <img width="99" alt="C# logo" src="../../../../_assets/logos/cs.svg">
    <br>
    <b>C#</b>
</h1>

[Home](../../../../README.md) / [C#](../../README.md) / [Chapter 02](./README.md)

---

# Conditionals, Switch, and Pattern Matching

> Control flow is where business rules become code.

**You will learn:**
- `if`, `else if`, and if-without-else patterns
- Ternary operator for short expressions
- `switch` statements and modern switch expressions

**Before this page, you should know:** method basics.

---

## Core patterns

```csharp
if (score >= 90) grade = "A";

if (isArchived)
    return;

if (score >= 80)
    grade = "B";
else if (score >= 70)
    grade = "C";
else
    grade = "D";
```

Ternary for short value assignment:

```csharp
string status = isActive ? "Active" : "Inactive";
```

Switch expression:

```csharp
string sizeLabel = size switch
{
    < 10 => "Small",
    < 100 => "Medium",
    _ => "Large"
};
```

Switch statement form is also valid when you need statement blocks:

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

## Nested conditionals

Nested branches are sometimes needed, but keep them shallow:

```csharp
if (user != null)
{
    if (user.IsActive)
        GrantAccess();
    else
        DenyAccess();
}
```

If nesting grows, extract helper methods with meaningful names.

## Pattern matching example

```csharp
string Describe(object value) => value switch
{
    int n when n < 0 => "Negative integer",
    int _ => "Integer",
    string s => $"Text({s.Length})",
    null => "Null",
    _ => "Unknown"
};
```

## Common control-flow mistakes

- Using ternary for large logic blocks
- Copy/paste if-chains that should be switch expressions
- Missing default case behavior
- Deep nesting that hides business intent

---

## Recap

- Use full `if/else if/else` chains for clear branching.
- Ternary is best for small expression-level choices.
- Switch expressions reduce repetitive branch code.

## Try it yourself

Implement a switch expression that maps HTTP status codes to labels.

---

[**Next ->** Loops and Iteration Patterns](./04-loops-and-iteration-patterns.md)  
[**<- Previous** Methods, Parameters, and Returns](./02-methods-parameters-and-returns.md)


