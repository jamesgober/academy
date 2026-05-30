<h1 align="center">
    <img width="99" alt="C# logo" src="../../../../_assets/logos/cs.svg">
    <br>
    <b>C#</b>
</h1>

[Home](../../../../README.md) / [C#](../../README.md) / [Chapter 03](./README.md)

---

# Constructors And Object Lifecycle

> Constructors are where an object earns the right to exist. If required data is
> missing or invalid, reject it before the object escapes into the rest of the
> program.

**You will learn:**
- What constructors do
- How constructor validation works
- How constructor chaining reduces duplication
- What object lifecycle means in managed C#
- How `IDisposable` and `using` fit into cleanup
- Why garbage collection is not the same as resource cleanup

**Before this page, you should know:** [Classes, Fields, And Properties](./01-classes-fields-and-properties.md)

---

## Constructor Basics

```csharp
public sealed class Order
{
    public Order(int id)
    {
        if (id <= 0)
        {
            throw new ArgumentOutOfRangeException(
                nameof(id),
                "Order id must be positive."
            );
        }

        Id = id;
        CreatedAt = DateTimeOffset.UtcNow;
    }

    public int Id { get; }
    public DateTimeOffset CreatedAt { get; }
}
```

This constructor guarantees:

```text
Every Order has a positive Id.
Every Order has a CreatedAt timestamp.
```

After construction, the object is valid.

---

## Constructor Parameters Are Required State

If a value is required for the object to make sense, make it a constructor
parameter.

Good:

```csharp
var order = new Order(1001);
```

Risky:

```csharp
var order = new Order();
order.Id = 1001;
```

The second version allows a temporary invalid object.

---

## Constructor Chaining

Use `this(...)` to route one constructor through another.

```csharp
public sealed class Order
{
    public Order(int id)
        : this(id, DateTimeOffset.UtcNow)
    {
    }

    public Order(int id, DateTimeOffset createdAt)
    {
        if (id <= 0)
        {
            throw new ArgumentOutOfRangeException(nameof(id));
        }

        Id = id;
        CreatedAt = createdAt;
    }

    public int Id { get; }
    public DateTimeOffset CreatedAt { get; }
}
```

Now validation exists in one place.

---

## Object Lifecycle

Basic managed object lifecycle:

```text
new object
  constructor runs
  object is used
  object becomes unreachable
  garbage collector eventually reclaims memory
```

C# manages memory for normal objects.

But some resources are not just memory:

- files
- sockets
- database connections
- streams
- timers
- OS handles

Those often need deterministic cleanup.

---

## `IDisposable` And `using`

Types that need cleanup often implement `IDisposable`.

```csharp
using var writer = new StreamWriter("log.txt");

writer.WriteLine("hello");
```

At the end of the scope, C# calls `Dispose()`.

Visual model:

```text
enter scope
  open file
  write line
leave scope
  Dispose closes file
```

This is similar in spirit to RAII in C++, but in C# you use `using` for
deterministic cleanup of disposable resources.

---

## Do Not Put Business Logic In Finalizers

C# has finalizers:

```csharp
~SomeType()
{
}
```

Beginners should almost never write them.

Finalizers are for advanced unmanaged resource cleanup scenarios. They run at an
unpredictable time and should not contain normal app logic.

---

## Real Example: Report Writer

```csharp
public sealed class Report
{
    private readonly List<string> _lines = new();

    public Report(string title)
    {
        if (string.IsNullOrWhiteSpace(title))
        {
            throw new ArgumentException("Title is required.", nameof(title));
        }

        Title = title;
    }

    public string Title { get; }

    public IReadOnlyList<string> Lines => _lines;

    public void AddLine(string line)
    {
        if (string.IsNullOrWhiteSpace(line))
        {
            return;
        }

        _lines.Add(line);
    }

    public async Task SaveAsync(string path)
    {
        await using var writer = new StreamWriter(path);

        await writer.WriteLineAsync(Title);

        foreach (string line in _lines)
        {
            await writer.WriteLineAsync(line);
        }
    }
}
```

Notice:

- constructor requires a title
- private list protects storage
- public read-only view exposes lines safely
- async file writer is disposed with `await using`

---

## Common Mistakes

### Mistake 1: Optional Everything

If everything has a public setter, callers can forget required values.

### Mistake 2: Validation After Construction

This is weak:

```csharp
var order = new Order();
order.Validate();
```

Prefer making invalid construction impossible.

### Mistake 3: Assuming GC Closes Files Immediately

Garbage collection reclaims memory eventually. Use `using` to close files and
streams predictably.

---

## Chapter Checkpoint

You should now be able to answer:

- What does a constructor do?
- Why should constructors validate required state?
- What does constructor chaining do?
- What does garbage collection manage?
- What does `IDisposable` represent?
- Why use `using` with files and streams?
- Why should beginners avoid finalizers?

---

## Recap

- Constructors create valid objects.
- Required state belongs in constructor parameters.
- Constructor chaining reduces duplication.
- C# manages normal memory with garbage collection.
- Disposable resources still need deterministic cleanup.
- `using` calls `Dispose()` at the end of scope.

## Try It Yourself

Create a `Customer` class:

- constructor requires `id` and `email`
- `id` must be positive
- `email` cannot be blank
- outside code can read both
- outside code cannot change `id`

---

[**Next ->** Interfaces, Inheritance, And Polymorphism](./03-interfaces-inheritance-and-polymorphism.md)  
[**<- Previous** Classes, Fields, And Properties](./01-classes-fields-and-properties.md)
