# Collections And LINQ Patterns

[C# Reference](./README.md)

---

## Related Lessons

- [Generic Collections In Practice](../course/04-collections-exceptions-and-data/01-generic-collections-in-practice.md)
- [Exception Handling And Failure Design](../course/04-collections-exceptions-and-data/02-exception-handling-and-failure-design.md)
- [LINQ Query Patterns](../course/04-collections-exceptions-and-data/03-linq-query-patterns.md)
- [Files And JSON Basics](../course/04-collections-exceptions-and-data/04-files-and-json-basics.md)
- [Chapter 04 Checkpoint](../course/04-collections-exceptions-and-data/05-chapter-04-checkpoint.md)

---

## Collection Selection

| Need | Collection | Example |
|---|---|---|
| ordered sequence | `List<T>` | order items |
| key lookup | `Dictionary<TKey,TValue>` | product by SKU |
| uniqueness | `HashSet<T>` | tags |
| first-in-first-out | `Queue<T>` | jobs to process |
| last-in-first-out | `Stack<T>` | undo history |
| read-only public view | `IReadOnlyList<T>` / `IReadOnlyCollection<T>` | expose private list safely |

---

## `List<T>`

```csharp
var names = new List<string>();
names.Add("Ada");
names.Remove("Ada");
int count = names.Count;
```

Use when:

- order matters
- duplicates are allowed
- you often loop through all items

Notice:

```text
names[index] throws if index is outside the list.
```

---

## `Dictionary<TKey,TValue>`

```csharp
var productBySku = new Dictionary<string, Product>();
productBySku.TryAdd(product.Sku, product);
```

Safe lookup:

```csharp
if (productBySku.TryGetValue("KB-100", out Product? product))
{
    Console.WriteLine(product.Name);
}
```

Notice:

```text
dictionary[key] throws when the key is missing.
Use TryGetValue when missing keys are normal.
```

---

## `HashSet<T>`

```csharp
var tags = new HashSet<string>();
tags.Add("csharp");
tags.Add("csharp");

Console.WriteLine(tags.Count); // 1
```

Use when uniqueness is the point.

---

## LINQ Quick Reference

| Method | Purpose | Example |
|---|---|---|
| `Where` | filter | `items.Where(x => x.Active)` |
| `Select` | transform | `items.Select(x => x.Name)` |
| `OrderBy` | sort ascending | `items.OrderBy(x => x.Name)` |
| `OrderByDescending` | sort descending | `items.OrderByDescending(x => x.Price)` |
| `FirstOrDefault` | first match or default | `items.FirstOrDefault(x => x.Id == id)` |
| `Any` | at least one match | `items.Any(x => x.Invalid)` |
| `All` | every item matches | `items.All(x => x.Price >= 0)` |
| `Sum` | numeric total | `items.Sum(x => x.Price)` |
| `GroupBy` | group by key | `items.GroupBy(x => x.Category)` |
| `ToList` | materialize now | `query.ToList()` |

---

## Common Pipeline

```csharp
var names = products
    .Where(product => product.Stock > 0)
    .OrderBy(product => product.Name)
    .Select(product => product.Name)
    .ToList();
```

Read:

```text
keep in-stock products
sort by name
turn products into names
store the results now
```

---

## Deferred Execution

This does not run immediately:

```csharp
var query = products.Where(product => product.Stock > 0);
```

It runs when enumerated:

```csharp
foreach (Product product in query)
{
    Console.WriteLine(product.Name);
}
```

Or materialized:

```csharp
List<Product> snapshot = query.ToList();
```

Notice:

```text
Use ToList when you need a stable snapshot or want to avoid re-running a query.
```

---

## Risk Notices

- Long LINQ chains can become harder to read than loops.
- `First()` throws when no item exists.
- `FirstOrDefault()` can return null for reference types.
- Deferred queries can re-run if enumerated multiple times.
- Returning `List<T>` publicly may allow callers to mutate your internals.

---

[C# Reference](./README.md)
