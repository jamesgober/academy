<h1 align="center">
    <img width="99" alt="C# logo" src="../../../../_assets/logos/cs.svg">
    <br>
    <b>C#</b>
</h1>

[Home](../../../../README.md) / [C#](../../README.md) / [Chapter 04](./README.md)

---

# Generic Collections In Practice

> A collection choice is a design choice. `List<T>`, `Dictionary<TKey,TValue>`,
> and `HashSet<T>` all store multiple values, but they make different operations
> easy.

**You will learn:**
- When to use `List<T>`
- When to use `Dictionary<TKey,TValue>`
- When to use `HashSet<T>`
- How `TryGetValue` works
- How to avoid duplicate data
- How to choose collections from real access patterns

**Before this page, you should know:** [Loops And Iteration Patterns](../02-core-language-basics/04-loops-and-iteration-patterns.md)

---

## Collection Decision Table

| Need | Use |
|---|---|
| ordered sequence | `List<T>` |
| lookup by key | `Dictionary<TKey,TValue>` |
| uniqueness check | `HashSet<T>` |
| queue behavior | `Queue<T>` |
| stack behavior | `Stack<T>` |
| read-only public view | `IReadOnlyList<T>` or `IReadOnlyCollection<T>` |

Do not choose collections by habit. Choose by the question your code asks most.

---

## `List<T>`

Use a list when order matters and you usually process items by looping.

```csharp
var names = new List<string>();

names.Add("Ada");
names.Add("Grace");
names.Add("Linus");

foreach (string name in names)
{
    Console.WriteLine(name);
}
```

Common operations:

| Operation | Example |
|---|---|
| add | `names.Add("Ada")` |
| remove by value | `names.Remove("Ada")` |
| remove by index | `names.RemoveAt(0)` |
| count | `names.Count` |
| index access | `names[0]` |
| clear | `names.Clear()` |

Risk notice:

```text
Index access throws if the index is outside the list.
```

---

## `Dictionary<TKey,TValue>`

Use a dictionary when you need fast lookup by key.

```csharp
var priceBySku = new Dictionary<string, decimal>();

priceBySku["KB-100"] = 49.99m;
priceBySku["MS-200"] = 19.99m;
```

Read safely:

```csharp
if (priceBySku.TryGetValue("KB-100", out decimal price))
{
    Console.WriteLine(price);
}
else
{
    Console.WriteLine("SKU not found.");
}
```

Avoid this when the key may be missing:

```csharp
decimal price = priceBySku["NOPE"];
```

That throws `KeyNotFoundException`.

---

## `HashSet<T>`

Use a set when uniqueness matters.

```csharp
var visitedIds = new HashSet<int>();

visitedIds.Add(10);
visitedIds.Add(10);
visitedIds.Add(20);

Console.WriteLine(visitedIds.Count); // 2
```

Check membership:

```csharp
if (visitedIds.Contains(20))
{
    Console.WriteLine("already visited");
}
```

Set operations:

```csharp
var admins = new HashSet<string> { "ada", "grace" };
var online = new HashSet<string> { "ada", "linus" };

admins.IntersectWith(online);
```

Now `admins` contains only users who were both admins and online.

---

## Real Example: Product Catalog

```csharp
public sealed class Product
{
    public Product(string sku, string name, decimal price)
    {
        if (string.IsNullOrWhiteSpace(sku))
        {
            throw new ArgumentException("SKU is required.", nameof(sku));
        }

        if (string.IsNullOrWhiteSpace(name))
        {
            throw new ArgumentException("Name is required.", nameof(name));
        }

        if (price < 0)
        {
            throw new ArgumentOutOfRangeException(nameof(price));
        }

        Sku = sku;
        Name = name;
        Price = price;
    }

    public string Sku { get; }
    public string Name { get; }
    public decimal Price { get; }
}

public sealed class ProductCatalog
{
    private readonly Dictionary<string, Product> _productsBySku = new();

    public bool Add(Product product)
    {
        return _productsBySku.TryAdd(product.Sku, product);
    }

    public Product? FindBySku(string sku)
    {
        if (_productsBySku.TryGetValue(sku, out Product? product))
        {
            return product;
        }

        return null;
    }

    public IReadOnlyCollection<Product> All()
    {
        return _productsBySku.Values;
    }
}
```

Use:

```csharp
var catalog = new ProductCatalog();

catalog.Add(new Product("KB-100", "Keyboard", 49.99m));
catalog.Add(new Product("MS-200", "Mouse", 19.99m));

Product? keyboard = catalog.FindBySku("KB-100");

if (keyboard is not null)
{
    Console.WriteLine(keyboard.Name);
}
```

---

## `IReadOnlyCollection<T>` And Friends

Returning a mutable list can expose internals:

```csharp
public List<Product> Products => _products;
```

Outside code could clear it:

```csharp
catalog.Products.Clear();
```

Prefer read-only views:

```csharp
public IReadOnlyList<Product> Products => _products;
```

This does not make the objects inside immutable, but it prevents callers from
adding/removing through that property.

---

## Common Mistakes

### Mistake 1: List Lookup Everywhere

If you constantly do this:

```csharp
products.FirstOrDefault(p => p.Sku == sku);
```

and SKU lookup is central, consider a dictionary.

### Mistake 2: Dictionary Indexer For Missing Keys

```csharp
var product = productsBySku[sku];
```

Only use this when missing keys are truly impossible. Otherwise use
`TryGetValue`.

### Mistake 3: Exposing Mutable Internals

Avoid returning private lists directly as `List<T>` when callers should not
mutate them.

---

## Mini Project: Unique Tags

Build a `TagList` class:

- stores tags uniquely
- ignores blank tags
- normalizes tags to lowercase
- can check whether a tag exists
- can return all tags

Starter:

```csharp
public sealed class TagList
{
    private readonly HashSet<string> _tags = new();

    public bool Add(string tag)
    {
        if (string.IsNullOrWhiteSpace(tag))
        {
            return false;
        }

        return _tags.Add(tag.Trim().ToLowerInvariant());
    }

    public bool Contains(string tag)
    {
        return _tags.Contains(tag.Trim().ToLowerInvariant());
    }

    public IReadOnlyCollection<string> All()
    {
        return _tags;
    }
}
```

---

## Chapter Checkpoint

You should now be able to answer:

- When should you use `List<T>`?
- When should you use `Dictionary<TKey,TValue>`?
- When should you use `HashSet<T>`?
- What does `TryGetValue` prevent?
- Why can returning `List<T>` expose too much power?
- What collection would you use for unique tags?

---

## Recap

- `List<T>` is for ordered sequences.
- `Dictionary<TKey,TValue>` is for key lookup.
- `HashSet<T>` is for uniqueness.
- `TryGetValue` handles missing keys without exceptions.
- Read-only interfaces protect collection ownership boundaries.

## Try It Yourself

Implement a product lookup by SKU using `Dictionary<string, Product>`.

Add tests or manual checks for:

- add product
- reject duplicate SKU
- find existing SKU
- missing SKU returns `null`

---

[**Next ->** Exception Handling And Failure Design](./02-exception-handling-and-failure-design.md)  
[**<- Previous** Chapter Start](./README.md)
