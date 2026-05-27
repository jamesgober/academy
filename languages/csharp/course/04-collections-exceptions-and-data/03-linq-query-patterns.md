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

# LINQ Query Patterns

> LINQ turns collection filtering and projection into readable pipelines.

**You will learn:**
- `Where`, `Select`, and `OrderBy`
- Materialization with `ToList`
- Common readability pitfalls in long query chains

**Before this page, you should know:** generic collections.

---

## Core LINQ flow

```csharp
var expensiveNames = products
    .Where(p => p.Price >= 50m)
    .OrderBy(p => p.Name)
    .Select(p => p.Name)
    .ToList();
```

Remember:
- LINQ is deferred until enumeration unless materialized.
- `ToList()` executes query now and stores results.

## Query expression option

```csharp
var ids = from p in products
          where p.Stock > 0
          select p.Id;
```

Method syntax is more common in modern codebases, but both are valid.

---

## Recap

- Use LINQ for clear transformation pipelines.
- Materialize when you need stable snapshot results.
- Keep chains readable with line breaks.

## Try it yourself

Filter a collection to active items, project to names, and sort alphabetically.

---

<!-- ===== FOOT NAV ===== -->
<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Exception Handling and Failure Design](./02-exception-handling-and-failure-design.md) | [Chapter](./README.md) · [Track](../../README.md) · [Home](../../../../README.md) | [Files and JSON Basics →](./04-files-and-json-basics.md) |

</div>
