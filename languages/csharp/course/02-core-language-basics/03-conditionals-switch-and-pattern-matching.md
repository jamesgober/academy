<h1 align="center">
    <img width="99" alt="C# logo" src="../../../../_assets/logos/cs.svg">
    <br>
    <b>C#</b>
</h1>

<!-- ===== HEAD NAV ===== -->
<div align="center">

[Home](../../../../README.md) · [C#](../../README.md) · [Chapter 02](./README.md)

</div>

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

---

## Recap

- Use full `if/else if/else` chains for clear branching.
- Ternary is best for small expression-level choices.
- Switch expressions reduce repetitive branch code.

## Try it yourself

Implement a switch expression that maps HTTP status codes to labels.

---

<!-- ===== FOOT NAV ===== -->
<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Methods, Parameters, and Returns](./02-methods-parameters-and-returns.md) | [Chapter](./README.md) · [Track](../../README.md) · [Home](../../../../README.md) | [Loops and Iteration Patterns →](./04-loops-and-iteration-patterns.md) |

</div>
