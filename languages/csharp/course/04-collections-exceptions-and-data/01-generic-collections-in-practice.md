<h1 align="center">
    <img width="99" alt="C# logo" src="../../../../_assets/logos/cs.svg">
    <br>
    <b>C#</b>
</h1>

<!-- ===== HEAD NAV ===== -->
<div align="center">

[Home](../../../../README.md) · [C#](../../README.md) · [Chapter 04](./README.md)

</div>

---

# Generic Collections in Practice

> Pick collection types by lookup needs, ordering needs, and uniqueness constraints.

**You will learn:**
- When to use `List<T>`, `Dictionary<TKey,TValue>`, and `HashSet<T>`
- Common performance-minded collection choices
- How collection choice affects program simplicity

**Before this page, you should know:** loops and methods.

---

## Core collection choices

```csharp
var names = new List<string>();
var priceBySku = new Dictionary<string, decimal>();
var visitedIds = new HashSet<int>();
```

Use:
- `List<T>` for ordered sequences
- `Dictionary<TKey,TValue>` for fast key lookup
- `HashSet<T>` for uniqueness and fast contains checks

## Practical pattern

```csharp
if (!priceBySku.TryGetValue("SKU-42", out var price))
{
    price = 0m;
}
```

---

## Recap

- Collection choice should match access patterns.
- `Dictionary` and `HashSet` are key performance tools.
- Prefer explicit collection intent over habit.

## Try it yourself

Implement a product lookup by SKU using `Dictionary<string, Product>`.

---

<!-- ===== FOOT NAV ===== -->
<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Chapter Start](./README.md) | [Chapter](./README.md) · [Track](../../README.md) · [Home](../../../../README.md) | [Exception Handling and Failure Design →](./02-exception-handling-and-failure-design.md) |

</div>
