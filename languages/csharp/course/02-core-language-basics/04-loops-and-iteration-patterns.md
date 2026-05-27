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

# Loops and Iteration Patterns

> Iteration is repetition with intent; choose the loop type that best matches your data.

**You will learn:**
- `for`, `while`, and `foreach` usage
- Why loop variable names matter
- Safe loop control with `break` and `continue`

**Before this page, you should know:** conditionals and switch.

---

## Loop choices

```csharp
for (int index = 0; index < items.Count; index++)
{
    Console.WriteLine(items[index]);
}

foreach (var item in items)
{
    Console.WriteLine(item);
}
```

Use `foreach` when you do not need indexes.
Use `for` when index math is part of logic.

## Naming loop variables

`i` is valid, but descriptive names are clearer in real projects:

```csharp
for (int productIndex = 0; productIndex < products.Count; productIndex++)
{
    // ...
}
```

## Control flow

- `break` exits loop immediately
- `continue` skips current iteration

---

## Recap

- Pick loops by intent, not habit.
- Prefer readable loop variable names.
- Use `break` and `continue` intentionally.

## Try it yourself

Rewrite one `for` loop into `foreach`, then explain which version is easier to read.

---

<!-- ===== FOOT NAV ===== -->
<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Conditionals, Switch, and Pattern Matching](./03-conditionals-switch-and-pattern-matching.md) | [Chapter](./README.md) · [Track](../../README.md) · [Home](../../../../README.md) | [Chapter 02 Checkpoint →](./05-chapter-02-checkpoint.md) |

</div>
