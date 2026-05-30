# OOP And Type Design Checklist

[C# Reference](./README.md)

---

## Related Lessons

- [Classes, Fields, And Properties](../course/03-classes-and-oop/01-classes-fields-and-properties.md)
- [Constructors And Object Lifecycle](../course/03-classes-and-oop/02-constructors-and-object-lifecycle.md)
- [Interfaces, Inheritance, And Polymorphism](../course/03-classes-and-oop/03-interfaces-inheritance-and-polymorphism.md)
- [Records, Structs, And When To Use Them](../course/03-classes-and-oop/04-records-structs-and-when-to-use-them.md)
- [Chapter 03 Checkpoint](../course/03-classes-and-oop/05-chapter-03-checkpoint.md)

---

## Type Selection

| Need | Choose | Notice |
|---|---|---|
| rich domain behavior with identity | `class` | default for entities/services |
| simple immutable data | `record` | value equality |
| JSON/API/file transfer shape | `record` DTO | keep domain clean |
| small immutable value | `readonly struct` or `readonly record struct` | avoid large structs |
| behavior contract | `interface` | great for substitution/testing |
| shared base behavior/state | abstract class | use sparingly |

---

## Class Checklist

- required state is provided in the constructor
- invalid state is rejected early
- fields are private
- public properties do not bypass rules
- mutation happens through named methods
- class has one clear job
- public API is smaller than internal implementation

Example:

```csharp
public sealed class BankAccount
{
    public BankAccount(string owner)
    {
        if (string.IsNullOrWhiteSpace(owner))
        {
            throw new ArgumentException("Owner is required.", nameof(owner));
        }

        Owner = owner;
    }

    public string Owner { get; }
    public decimal Balance { get; private set; }

    public void Deposit(decimal amount)
    {
        if (amount <= 0)
        {
            throw new ArgumentOutOfRangeException(nameof(amount));
        }

        Balance += amount;
    }
}
```

---

## Property Checklist

Use:

```csharp
public string Name { get; private set; }
```

when outside code can read but should not directly change.

Use:

```csharp
public string Name { get; init; }
```

for data set during construction/object initialization.

Use a full property when validation is needed:

```csharp
private decimal _price;

public decimal Price
{
    get => _price;
    private set
    {
        if (value < 0)
        {
            throw new ArgumentOutOfRangeException(nameof(value));
        }

        _price = value;
    }
}
```

---

## Interface Checklist

Use interfaces when:

- code needs multiple implementations
- tests need fakes
- a boundary should not depend on a concrete class
- the contract is small and cohesive

Example:

```csharp
public interface INotifier
{
    Task SendAsync(string message);
}
```

Risk notices:

- Do not create interfaces for every class automatically.
- Avoid giant interfaces.
- Do not hide poor design behind unnecessary abstraction.

---

## Record Checklist

Use records for:

- DTOs
- settings
- messages
- simple value-like models

Example:

```csharp
public sealed record OrderItemDto(
    string Sku,
    int Quantity,
    decimal UnitPrice
);
```

Notices:

- records compare by values
- `with` creates a modified copy
- mutable objects inside records can still be shared
- records are not automatically deeply immutable

---

## Struct Checklist

Use structs only when the type is:

- small
- immutable
- value-like
- frequently created

Prefer:

```csharp
public readonly record struct Pixel(int X, int Y);
```

Avoid mutable structs in beginner projects.

---

## Failure Design

Throw exceptions for broken method contracts:

```csharp
throw new ArgumentOutOfRangeException(nameof(quantity));
```

Return `false` or `null` for expected alternate outcomes:

```csharp
public Product? FindBySku(string sku);
public bool TryAdd(Product product);
```

---

## Review Questions

- Can this object be invalid after construction?
- Can outside code mutate internals directly?
- Is this a class because identity matters?
- Is this a record because values matter?
- Is this interface used at a real substitution point?
- Is a collection exposed as mutable when it should be read-only?

---

[C# Reference](./README.md)
