<h1 align="center">
    <img width="99" alt="C# logo" src="../../../../_assets/logos/cs.svg">
    <br>
    <b>C#</b>
</h1>

[Home](../../../../README.md) / [C#](../../README.md) / [Chapter 03](./README.md)

---

# Records, Structs, And When To Use Them

> C# gives you several ways to model data. The right choice affects equality,
> copying, mutation, memory behavior, and how readable your design feels.

**You will learn:**
- When to choose `class`
- When to choose `record`
- When to choose `struct`
- What value equality means
- How `with` expressions work
- Why mutable structs are dangerous
- How to choose types for real app models

**Before this page, you should know:** [Interfaces, Inheritance, And Polymorphism](./03-interfaces-inheritance-and-polymorphism.md)

---

## The Short Version

| Type shape | Best for | Equality default |
|---|---|---|
| `class` | rich domain objects with identity and behavior | same object reference |
| `record class` / `record` | immutable data models and DTOs | same values |
| `struct` | small value types | value-like, but use carefully |
| `record struct` | small value data with record features | same values |

Beginner default:

```text
Use class for behavior-rich domain objects.
Use record for simple immutable data.
Avoid custom structs until you have a clear reason.
```

---

## Class Equality

Two class instances with the same values are usually not equal by default.

```csharp
public sealed class AddressClass
{
    public AddressClass(string city, string country)
    {
        City = city;
        Country = country;
    }

    public string City { get; }
    public string Country { get; }
}

var first = new AddressClass("Paris", "France");
var second = new AddressClass("Paris", "France");

Console.WriteLine(first == second); // False
```

Why?

```text
first and second are two different objects.
```

This is useful for domain entities where identity matters.

Example:

```text
Two orders may have the same total, but they are not the same order.
```

---

## Record Equality

Records compare by values.

```csharp
public sealed record Address(string City, string Country);

var first = new Address("Paris", "France");
var second = new Address("Paris", "France");

Console.WriteLine(first == second); // True
```

Plain language:

```text
Records ask: do these values match?
Classes ask by default: is this the same object?
```

Records are excellent for:

- DTOs
- settings
- messages
- API request/response models
- immutable value-style data

---

## Positional Records

```csharp
public sealed record Money(string Currency, decimal Amount);
```

This creates:

- constructor
- public init-only properties
- value equality
- useful `ToString`
- deconstruction support

Use:

```csharp
var price = new Money("USD", 19.99m);

Console.WriteLine(price.Currency);
Console.WriteLine(price.Amount);
```

---

## Record With Validation

For validation, write a record with a constructor body.

```csharp
public sealed record Money
{
    public Money(string currency, decimal amount)
    {
        if (string.IsNullOrWhiteSpace(currency))
        {
            throw new ArgumentException("Currency is required.", nameof(currency));
        }

        if (amount < 0)
        {
            throw new ArgumentOutOfRangeException(
                nameof(amount),
                "Amount cannot be negative."
            );
        }

        Currency = currency;
        Amount = amount;
    }

    public string Currency { get; init; }
    public decimal Amount { get; init; }
}
```

This keeps record benefits while protecting rules.

---

## `with` Expressions

Records are often used immutably.

```csharp
var original = new Money("USD", 10m);
var discounted = original with { Amount = 8m };
```

Now:

```text
original.Amount is 10
discounted.Amount is 8
```

The `with` expression creates a copy with selected changes.

This is great for value-style data.

Risk notice:

```text
with copies the record object, but it does not deeply clone every object inside.
If a record contains a mutable list, both records can still refer to the same list.
```

---

## Structs

A struct is a value type.

```csharp
public readonly struct Point
{
    public Point(int x, int y)
    {
        X = x;
        Y = y;
    }

    public int X { get; }
    public int Y { get; }
}
```

Structs are copied by value.

```csharp
Point a = new Point(1, 2);
Point b = a;
```

`b` gets a copy.

Use structs for small, immutable, value-like things:

- coordinates
- measurements
- tiny numeric concepts

Avoid structs for rich domain objects with identity or lots of fields.

---

## Mutable Struct Trap

Mutable structs are confusing because copies can be changed independently.

```csharp
public struct Counter
{
    public int Value { get; set; }
}

var first = new Counter { Value = 1 };
var second = first;

second.Value = 99;

Console.WriteLine(first.Value);  // 1
Console.WriteLine(second.Value); // 99
```

The copy changed, not the original.

Beginner rule:

```text
If you write a struct, make it readonly unless you know exactly why not.
```

---

## Record Struct

```csharp
public readonly record struct Pixel(int X, int Y);
```

This gives value-type behavior plus record conveniences.

Use for small immutable data where allocation and value semantics matter.

Do not use record structs just because they look concise. If the type can grow
behavior, identity, or many fields, a class may be clearer.

---

## Real Example: Order Types

```csharp
public sealed record OrderItem(
    string Sku,
    int Quantity,
    decimal UnitPrice
);

public sealed class Order
{
    private readonly List<OrderItem> _items = new();

    public Order(int id)
    {
        if (id <= 0)
        {
            throw new ArgumentOutOfRangeException(nameof(id));
        }

        Id = id;
    }

    public int Id { get; }
    public IReadOnlyList<OrderItem> Items => _items;
    public decimal Total => _items.Sum(item => item.Quantity * item.UnitPrice);

    public void AddItem(OrderItem item)
    {
        _items.Add(item);
    }
}
```

Why:

```text
OrderItem is value-like data, so record is good.
Order has identity and behavior, so class is good.
```

---

## Decision Table

| Situation | Choose |
|---|---|
| user account with id and behavior | class |
| order with line items and methods | class |
| JSON DTO | record |
| settings object | record |
| command message | record |
| small coordinate | readonly struct or readonly record struct |
| mutable app service | class |

---

## Common Mistakes

### Mistake 1: Records With Mutable Lists

```csharp
public record OrderDto(List<OrderItemDto> Items);
```

This is okay for DTOs, but remember the list itself is mutable.

If you need true immutability, use immutable collections or copy defensively.

### Mistake 2: Structs For Big Domain Objects

Large structs can copy more data than you expect.

Use classes for rich domain models.

### Mistake 3: Record For Identity Objects

If two objects with the same values should still be different things, use a
class.

---

## Chapter Checkpoint

You should now be able to answer:

- What is the default equality behavior for classes?
- What is value equality?
- Why are records useful for DTOs?
- What does `with` do?
- Why can mutable lists inside records be risky?
- Why should beginner structs usually be immutable?
- When should an order be a class instead of a record?

---

## Recap

- Classes are best for rich behavior and identity.
- Records are best for value-like data and DTOs.
- Structs are value types and should usually be small and immutable.
- Record equality compares values.
- Class equality defaults to object identity.
- `with` creates a modified copy of a record.

## Try It Yourself

Model:

- `Address` as a record
- `OrderItem` as a record
- `Order` as a class

Then explain why each choice fits.

---

[**Next ->** Chapter 03 Checkpoint](./05-chapter-03-checkpoint.md)  
[**<- Previous** Interfaces, Inheritance, And Polymorphism](./03-interfaces-inheritance-and-polymorphism.md)
