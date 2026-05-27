<h1 align="center">
    <img width="99" alt="C# logo" src="../../../../_assets/logos/cs.svg">
    <br>
    <b>C#</b>
</h1>

<!-- ===== HEAD NAV ===== -->
<div align="center">

[Home](../../../../README.md) · [C#](../../README.md) · [Chapter 03](./README.md)

</div>

---

# Constructors and Object Lifecycle

> Constructors create valid objects from the start.

**You will learn:**
- Constructor parameters and validation
- Constructor chaining with `this(...)`
- Object lifecycle basics in managed memory

**Before this page, you should know:** fields and properties.

---

## Constructor example

```csharp
public class Order
{
    public int Id { get; }
    public DateTime CreatedAt { get; }

    public Order(int id) : this(id, DateTime.UtcNow) { }

    public Order(int id, DateTime createdAt)
    {
        if (id <= 0) throw new ArgumentOutOfRangeException(nameof(id));
        Id = id;
        CreatedAt = createdAt;
    }
}
```

Constructor goals:
- enforce required state
- reject invalid input early
- avoid partially initialized objects

## Lifecycle note

C# uses garbage collection, but deterministic cleanup still matters for unmanaged resources (`IDisposable`, `using`).

---

## Recap

- Constructors enforce object validity.
- Use constructor chaining to reduce duplication.
- Managed memory does not remove all resource-management responsibility.

## Try it yourself

Add validation and constructor overloads to one of your classes.

---

<!-- ===== FOOT NAV ===== -->
<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Classes, Fields, and Properties](./01-classes-fields-and-properties.md) | [Chapter](./README.md) · [Track](../../README.md) · [Home](../../../../README.md) | [Interfaces, Inheritance, and Polymorphism →](./03-interfaces-inheritance-and-polymorphism.md) |

</div>
