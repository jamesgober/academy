# Collections and LINQ Patterns

## Collection selection

- `List<T>`: ordered sequence
- `Dictionary<TKey, TValue>`: key lookup
- `HashSet<T>`: uniqueness and membership checks
- `Queue<T>` / `Stack<T>`: FIFO / LIFO workflows

## Basic LINQ operations

```csharp
var q = items
    .Where(x => x.IsActive)
    .OrderBy(x => x.Name)
    .Select(x => x.Name)
    .ToList();
```

## Common patterns

```csharp
var grouped = items.GroupBy(x => x.Category);
var first = items.FirstOrDefault(x => x.Id == targetId);
var anyInvalid = items.Any(x => x.Amount < 0);
```

## Caution

- Do not over-chain queries if readability suffers.
- Materialize (`ToList`) when you need stable snapshots.

---

[← C# Reference](./README.md)
