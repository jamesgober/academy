<h1 align="center">
    <img width="99" alt="C# logo" src="../../../../_assets/logos/cs.svg">
    <br>
    <b>C#</b>
</h1>

[Home](../../../../README.md) / [C#](../../README.md) / [Chapter 04](./README.md)

---

# LINQ Query Patterns

> LINQ lets you describe collection transformations as readable pipelines:
> filter, sort, map, group, and materialize.

**You will learn:**
- `Where`
- `Select`
- `OrderBy`
- `FirstOrDefault`
- `Any` and `All`
- `GroupBy`
- `Sum`
- deferred execution
- when to use `ToList`

**Before this page, you should know:** [Generic Collections In Practice](./01-generic-collections-in-practice.md)

---

## Sample Data

```csharp
public sealed record Product(
    string Sku,
    string Name,
    string Category,
    decimal Price,
    int Stock
);

var products = new List<Product>
{
    new("KB-100", "Keyboard", "Peripherals", 49.99m, 10),
    new("MS-200", "Mouse", "Peripherals", 19.99m, 0),
    new("MN-300", "Monitor", "Displays", 199.99m, 4),
    new("HD-400", "Headphones", "Audio", 79.99m, 8)
};
```

---

## `Where`: Filter

```csharp
var inStock = products
    .Where(product => product.Stock > 0)
    .ToList();
```

Plain language:

```text
Keep only products where Stock is greater than 0.
```

---

## `Select`: Transform

```csharp
var names = products
    .Select(product => product.Name)
    .ToList();
```

Plain language:

```text
Turn each Product into its Name.
```

Projection to another shape:

```csharp
var labels = products
    .Select(product => $"{product.Sku}: {product.Name}")
    .ToList();
```

---

## `OrderBy` And `ThenBy`

```csharp
var sorted = products
    .OrderBy(product => product.Category)
    .ThenBy(product => product.Name)
    .ToList();
```

Descending:

```csharp
var expensiveFirst = products
    .OrderByDescending(product => product.Price)
    .ToList();
```

---

## `FirstOrDefault`

```csharp
Product? keyboard = products
    .FirstOrDefault(product => product.Sku == "KB-100");
```

Check for `null`:

```csharp
if (keyboard is not null)
{
    Console.WriteLine(keyboard.Name);
}
```

Risk notice:

```text
First() throws when nothing matches.
FirstOrDefault() returns null for reference types when nothing matches.
```

---

## `Any` And `All`

```csharp
bool hasOutOfStock = products.Any(product => product.Stock == 0);
```

```csharp
bool allHavePrices = products.All(product => product.Price >= 0);
```

Use these when you need a yes/no question.

---

## `Sum`

```csharp
decimal inventoryValue = products
    .Sum(product => product.Price * product.Stock);
```

Plain language:

```text
For each product, multiply price by stock, then add all results.
```

---

## `GroupBy`

```csharp
var productsByCategory = products
    .GroupBy(product => product.Category);

foreach (var group in productsByCategory)
{
    Console.WriteLine(group.Key);

    foreach (Product product in group)
    {
        Console.WriteLine($"- {product.Name}");
    }
}
```

`group.Key` is the category.

`group` contains the products in that category.

---

## Deferred Execution

Many LINQ queries do not run immediately.

```csharp
var query = products.Where(product => product.Stock > 0);
```

The query runs when you enumerate it:

```csharp
foreach (Product product in query)
{
    Console.WriteLine(product.Name);
}
```

Or when you materialize it:

```csharp
List<Product> snapshot = query.ToList();
```

`ToList()` means:

```text
Run the query now and store the results.
```

---

## Real Example: Inventory Report

```csharp
var reportLines = products
    .Where(product => product.Stock > 0)
    .OrderBy(product => product.Category)
    .ThenBy(product => product.Name)
    .Select(product =>
        $"{product.Category} / {product.Name}: {product.Stock} in stock"
    )
    .ToList();

foreach (string line in reportLines)
{
    Console.WriteLine(line);
}
```

This reads as:

```text
from all products
keep in-stock products
sort by category and name
turn each product into a report line
store the result
print each line
```

---

## Common Mistakes

### Mistake 1: Giant One-Line Queries

Hard to read:

```csharp
var x = products.Where(p => p.Stock > 0).OrderBy(p => p.Name).Select(p => p.Name).ToList();
```

Better:

```csharp
var names = products
    .Where(product => product.Stock > 0)
    .OrderBy(product => product.Name)
    .Select(product => product.Name)
    .ToList();
```

### Mistake 2: Re-Running Expensive Queries

If you enumerate a deferred query many times, it may run many times.

Use `ToList()` when you need a stable snapshot.

### Mistake 3: Using LINQ When A Loop Is Clearer

LINQ is not mandatory. If a loop is easier to understand, use a loop.

---

## Mini Project: Product Report

Given products, create:

- list of in-stock names
- total inventory value
- most expensive product
- count by category

Hints:

```csharp
products.Where(...)
products.Sum(...)
products.OrderByDescending(...).FirstOrDefault()
products.GroupBy(...)
```

---

## Chapter Checkpoint

You should now be able to answer:

- What does `Where` do?
- What does `Select` do?
- What does `OrderBy` do?
- Why does `FirstOrDefault` require null checking?
- What does deferred execution mean?
- What does `ToList` do?
- When might a loop be clearer than LINQ?

---

## Recap

- LINQ creates readable collection pipelines.
- `Where` filters.
- `Select` transforms.
- `OrderBy` sorts.
- `Any` and `All` answer yes/no questions.
- `Sum` aggregates numbers.
- `GroupBy` creates groups.
- `ToList` materializes a query now.

## Try It Yourself

Filter a collection to active items, project to names, sort alphabetically, and
materialize the result with `ToList()`.

---

[**Next ->** Files And JSON Basics](./04-files-and-json-basics.md)  
[**<- Previous** Exception Handling And Failure Design](./02-exception-handling-and-failure-design.md)
