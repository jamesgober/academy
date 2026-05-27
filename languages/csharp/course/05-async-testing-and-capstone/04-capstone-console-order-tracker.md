<h1 align="center">
    <img width="99" alt="C# logo" src="../../../../_assets/logos/cs.svg">
    <br>
    <b>C#</b>
</h1>

<!-- ===== HEAD NAV ===== -->
<div align="center">

[Home](../../../../README.md) · [C#](../../README.md) · [Chapter 05](./README.md)

</div>

---

# Capstone: Console Order Tracker

> Build a small but realistic C# project that combines OOP, collections, async, and testing.

**You will learn:**
- How to shape a small project structure
- How to separate domain logic from IO
- How to define completion criteria

**Before this page, you should know:** all prior chapters.

---

## Suggested structure

```text
order-tracker/
├── src/
│   ├── OrderTracker.App/
│   └── OrderTracker.Core/
└── tests/
    └── OrderTracker.Core.Tests/
```

Recommended layering:
- `OrderTracker.Core`: domain models, services, validation
- `OrderTracker.App`: console input/output orchestration
- `OrderTracker.Core.Tests`: behavior tests for domain layer

## Features

- create orders
- add line items
- calculate totals
- save and load from JSON
- validate input and report clear errors

## Suggested domain sketch

```csharp
public record OrderItem(string Sku, int Quantity, decimal UnitPrice);

public class Order
{
    public int Id { get; }
    public List<OrderItem> Items { get; } = new();

    public Order(int id) => Id = id;
    public decimal Total() => Items.Sum(i => i.Quantity * i.UnitPrice);
}
```

## Delivery milestones

1. Build order model and total calculation.
2. Add commands to create order and add items.
3. Persist orders to JSON file.
4. Add tests for totals and input validation.
5. Add logging around load/save and failures.

## Expected output examples

- `Order created: 1001`
- `Item added: SKU-22 x3`
- `Current total: 74.97`
- `Saved 1 order(s) to orders.json`

## Completion checklist

- `dotnet build` clean
- `dotnet test` passing
- no unresolved warnings you do not understand
- one short architecture note explaining type choices

## Test plan baseline

- total calculation with 1+ line items
- validation: reject zero or negative quantities
- JSON round-trip: save then load retains totals
- invalid input path emits clear error message

---

## Recap

- Real projects combine multiple language features.
- Separation of concerns keeps code testable.
- Completion criteria must be explicit.

## Try it yourself

Implement one vertical slice: create order -> add line -> show total.

---

<!-- ===== FOOT NAV ===== -->
<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Debugging and Logging Workflow](./03-debugging-and-logging-workflow.md) | [Chapter](./README.md) · [Track](../../README.md) · [Home](../../../../README.md) | [Chapter 05 Final Checkpoint →](./05-chapter-05-final-checkpoint.md) |

</div>
