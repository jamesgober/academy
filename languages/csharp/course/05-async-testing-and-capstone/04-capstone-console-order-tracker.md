<h1 align="center">
    <img width="99" alt="C# logo" src="../../../../_assets/logos/cs.svg">
    <br>
    <b>C#</b>
</h1>

[Home](../../../../README.md) / [C#](../../README.md) / [Chapter 05](./README.md)

---

# Capstone: Console Order Tracker

> This project turns the C# course into a small piece of software. You will build
> a domain library, a console app, JSON persistence, async file IO, and tests.

**You will build:**
- order and line-item domain models
- an order service
- JSON save/load
- a console app
- xUnit tests
- a short architecture note

**Before this page, you should know:** all prior C# chapters.

---

## Final Project Shape

```text
order-tracker/
  src/
    OrderTracker.App/
      OrderTracker.App.csproj
      Program.cs
    OrderTracker.Core/
      OrderTracker.Core.csproj
      Order.cs
      OrderItem.cs
      OrderBook.cs
      JsonOrderStore.cs
  tests/
    OrderTracker.Core.Tests/
      OrderTracker.Core.Tests.csproj
      OrderTests.cs
```

Create it:

```powershell
mkdir order-tracker
cd order-tracker

dotnet new sln -n OrderTracker

mkdir src
mkdir tests

dotnet new classlib -n OrderTracker.Core -o src/OrderTracker.Core
dotnet new console -n OrderTracker.App -o src/OrderTracker.App
dotnet new xunit -n OrderTracker.Core.Tests -o tests/OrderTracker.Core.Tests

dotnet sln add src/OrderTracker.Core/OrderTracker.Core.csproj
dotnet sln add src/OrderTracker.App/OrderTracker.App.csproj
dotnet sln add tests/OrderTracker.Core.Tests/OrderTracker.Core.Tests.csproj

dotnet add src/OrderTracker.App/OrderTracker.App.csproj reference src/OrderTracker.Core/OrderTracker.Core.csproj
dotnet add tests/OrderTracker.Core.Tests/OrderTracker.Core.Tests.csproj reference src/OrderTracker.Core/OrderTracker.Core.csproj
```

---

## Step 1: Order Item

`OrderItem.cs`:

```csharp
namespace OrderTracker.Core;

public sealed record OrderItem
{
    public OrderItem(string sku, int quantity, decimal unitPrice)
    {
        if (string.IsNullOrWhiteSpace(sku))
        {
            throw new ArgumentException("SKU is required.", nameof(sku));
        }

        if (quantity <= 0)
        {
            throw new ArgumentOutOfRangeException(
                nameof(quantity),
                "Quantity must be positive."
            );
        }

        if (unitPrice < 0)
        {
            throw new ArgumentOutOfRangeException(
                nameof(unitPrice),
                "Unit price cannot be negative."
            );
        }

        Sku = sku;
        Quantity = quantity;
        UnitPrice = unitPrice;
    }

    public string Sku { get; init; }
    public int Quantity { get; init; }
    public decimal UnitPrice { get; init; }

    public decimal LineTotal => Quantity * UnitPrice;
}
```

Why a record?

```text
OrderItem is mostly data with validation and value-like behavior.
```

---

## Step 2: Order

`Order.cs`:

```csharp
namespace OrderTracker.Core;

public sealed class Order
{
    private readonly List<OrderItem> _items = new();

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
    }

    public int Id { get; }

    public IReadOnlyList<OrderItem> Items => _items;

    public decimal Total => _items.Sum(item => item.LineTotal);

    public void AddItem(OrderItem item)
    {
        _items.Add(item);
    }
}
```

Design notes:

```text
Order owns its private list.
Callers can read Items but cannot add directly.
AddItem is the controlled mutation point.
Total is calculated from current items.
```

---

## Step 3: Order Book

`OrderBook.cs`:

```csharp
namespace OrderTracker.Core;

public sealed class OrderBook
{
    private readonly Dictionary<int, Order> _ordersById = new();

    public bool Add(Order order)
    {
        return _ordersById.TryAdd(order.Id, order);
    }

    public Order? Find(int id)
    {
        return _ordersById.TryGetValue(id, out Order? order)
            ? order
            : null;
    }

    public IReadOnlyCollection<Order> All()
    {
        return _ordersById.Values;
    }
}
```

Why a dictionary?

```text
The main lookup question is "find order by id."
Dictionary<int, Order> matches that access pattern.
```

---

## Step 4: JSON Store With DTOs

`JsonOrderStore.cs`:

```csharp
using System.Text.Json;

namespace OrderTracker.Core;

public sealed class JsonOrderStore
{
    private readonly string _path;
    private readonly JsonSerializerOptions _options = new()
    {
        WriteIndented = true
    };

    public JsonOrderStore(string path)
    {
        _path = path;
    }

    public async Task SaveAsync(
        IReadOnlyCollection<Order> orders,
        CancellationToken cancellationToken = default
    )
    {
        List<OrderDto> dto = orders
            .Select(order => new OrderDto(
                order.Id,
                order.Items
                    .Select(item => new OrderItemDto(
                        item.Sku,
                        item.Quantity,
                        item.UnitPrice
                    ))
                    .ToList()
            ))
            .ToList();

        string json = JsonSerializer.Serialize(dto, _options);
        await File.WriteAllTextAsync(_path, json, cancellationToken);
    }

    public async Task<List<Order>> LoadAsync(
        CancellationToken cancellationToken = default
    )
    {
        if (!File.Exists(_path))
        {
            return new List<Order>();
        }

        string json = await File.ReadAllTextAsync(_path, cancellationToken);

        List<OrderDto> dto = JsonSerializer.Deserialize<List<OrderDto>>(
            json,
            _options
        ) ?? new List<OrderDto>();

        var orders = new List<Order>();

        foreach (OrderDto savedOrder in dto)
        {
            var order = new Order(savedOrder.Id);

            foreach (OrderItemDto savedItem in savedOrder.Items)
            {
                order.AddItem(new OrderItem(
                    savedItem.Sku,
                    savedItem.Quantity,
                    savedItem.UnitPrice
                ));
            }

            orders.Add(order);
        }

        return orders;
    }

    private sealed record OrderDto(int Id, List<OrderItemDto> Items);

    private sealed record OrderItemDto(
        string Sku,
        int Quantity,
        decimal UnitPrice
    );
}
```

Important beginner note:

```text
JsonOrderStore saves DTOs instead of directly saving domain objects.
That lets Order keep its private list while the file format stays simple.
```

---

## Step 5: Console App

`Program.cs`:

```csharp
using OrderTracker.Core;

var orders = new OrderBook();
var store = new JsonOrderStore("orders.json");

foreach (Order order in await store.LoadAsync())
{
    orders.Add(order);
}

Console.WriteLine("Order Tracker");
Console.WriteLine("Commands: new, add, show, save, exit");

while (true)
{
    Console.Write("> ");
    string? command = Console.ReadLine();

    if (command is null || command == "exit")
    {
        break;
    }

    if (command == "new")
    {
        Console.Write("Order id: ");
        int id = int.Parse(Console.ReadLine() ?? "0");

        var order = new Order(id);

        if (orders.Add(order))
        {
            Console.WriteLine($"Order created: {id}");
        }
        else
        {
            Console.WriteLine("Order already exists.");
        }
    }
    else if (command == "add")
    {
        Console.Write("Order id: ");
        int id = int.Parse(Console.ReadLine() ?? "0");

        Order? order = orders.Find(id);

        if (order is null)
        {
            Console.WriteLine("Order not found.");
            continue;
        }

        Console.Write("SKU: ");
        string sku = Console.ReadLine() ?? "";

        Console.Write("Quantity: ");
        int quantity = int.Parse(Console.ReadLine() ?? "0");

        Console.Write("Unit price: ");
        decimal unitPrice = decimal.Parse(Console.ReadLine() ?? "0");

        order.AddItem(new OrderItem(sku, quantity, unitPrice));
        Console.WriteLine("Item added.");
    }
    else if (command == "show")
    {
        foreach (Order order in orders.All())
        {
            Console.WriteLine($"Order {order.Id}: {order.Total:C}");

            foreach (OrderItem item in order.Items)
            {
                Console.WriteLine(
                    $"  {item.Sku} x{item.Quantity} @ {item.UnitPrice:C}"
                );
            }
        }
    }
    else if (command == "save")
    {
        await store.SaveAsync(orders.All());
        Console.WriteLine("Saved orders.");
    }
    else
    {
        Console.WriteLine("Unknown command.");
    }
}
```

This version is intentionally simple. Later, improve parsing so bad input does
not throw.

---

## Step 6: Tests

`OrderTests.cs`:

```csharp
using OrderTracker.Core;

namespace OrderTracker.Core.Tests;

public sealed class OrderTests
{
    [Fact]
    public void Total_AddsLineTotals()
    {
        var order = new Order(1001);

        order.AddItem(new OrderItem("KB-100", 2, 10m));
        order.AddItem(new OrderItem("MS-200", 1, 5m));

        Assert.Equal(25m, order.Total);
    }

    [Fact]
    public void OrderItem_RejectsZeroQuantity()
    {
        Assert.Throws<ArgumentOutOfRangeException>(
            () => new OrderItem("KB-100", 0, 10m)
        );
    }

    [Fact]
    public void OrderBook_RejectsDuplicateOrderIds()
    {
        var book = new OrderBook();

        Assert.True(book.Add(new Order(1001)));
        Assert.False(book.Add(new Order(1001)));
    }
}
```

Run:

```powershell
dotnet test
```

---

## Required Build Gates

```powershell
dotnet build
dotnet test
dotnet run --project src/OrderTracker.App
```

Before sign-off:

- build is clean
- tests pass
- bad quantities are rejected
- duplicate orders are rejected
- totals are correct
- JSON save works
- architecture note explains type choices

---

## Architecture Note

Create `ARCHITECTURE.md`:

```md
# Order Tracker Architecture

## Core

OrderTracker.Core contains domain rules: Order, OrderItem, OrderBook, and JSON
storage.

## App

OrderTracker.App handles console input and output. It should not own business
rules.

## Tests

OrderTracker.Core.Tests tests domain behavior without depending on console input.

## Collection Choices

OrderBook uses Dictionary<int, Order> because order lookup happens by id.
Order uses List<OrderItem> because item order is useful and duplicates are
allowed.

## Async

JsonOrderStore uses async file IO because file reads and writes wait on the
operating system.
```

---

## Stretch: Safer Console Parsing

Replace:

```csharp
int id = int.Parse(Console.ReadLine() ?? "0");
```

with:

```csharp
if (!int.TryParse(Console.ReadLine(), out int id))
{
    Console.WriteLine("Invalid id.");
    continue;
}
```

This turns bad user input into a friendly message instead of a crash.

---

## Completion Checklist

- solution and projects created
- project references connected
- domain models compile
- console app runs
- tests pass
- totals are correct
- validation rejects bad data
- async save works
- architecture note exists
- next improvement is identified

---

## Recap

- Real C# apps separate domain logic from IO.
- Collections should match lookup needs.
- Properties and methods protect rules.
- Async file IO waits efficiently.
- Tests should target domain behavior.
- Console parsing needs friendly validation.

## Try It Yourself

Implement one vertical slice:

```text
create order -> add item -> show total -> save orders
```

Then add one test before adding the next feature.

---

[**Next ->** Chapter 05 Final Checkpoint](./05-chapter-05-final-checkpoint.md)  
[**<- Previous** Debugging And Logging Workflow](./03-debugging-and-logging-workflow.md)
