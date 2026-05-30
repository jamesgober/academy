<h1 align="center">
    <img width="99" alt="C# logo" src="../../../../_assets/logos/cs.svg">
    <br>
    <b>C#</b>
</h1>

[Home](../../../../README.md) / [C#](../../README.md) / [Chapter 03](./README.md)

---

# Interfaces, Inheritance, And Polymorphism

> Interfaces are one of C#'s best tools for replaceable, testable code. They let
> you depend on what something can do instead of the exact class that does it.

**You will learn:**
- What interfaces are
- How polymorphism works through interface references
- When inheritance helps
- Why composition is often simpler
- How interfaces improve testing
- How to avoid over-design

**Before this page, you should know:** [Constructors And Object Lifecycle](./02-constructors-and-object-lifecycle.md)

---

## Interface Mental Model

An interface is a contract:

```text
Any type that implements this promises to provide these members.
```

Example:

```csharp
public interface INotifier
{
    Task SendAsync(string message);
}
```

This says:

```text
An INotifier can send a message asynchronously.
```

It does not say whether the message goes to email, SMS, console, Slack, or a
test fake.

---

## Implement An Interface

```csharp
public sealed class ConsoleNotifier : INotifier
{
    public Task SendAsync(string message)
    {
        Console.WriteLine($"[console] {message}");
        return Task.CompletedTask;
    }
}
```

Use:

```csharp
INotifier notifier = new ConsoleNotifier();
await notifier.SendAsync("Order shipped");
```

The variable type is `INotifier`.

The actual object is `ConsoleNotifier`.

That is polymorphism.

---

## Why Interfaces Help Testing

Suppose an order service sends a notification:

```csharp
public sealed class OrderService
{
    private readonly INotifier _notifier;

    public OrderService(INotifier notifier)
    {
        _notifier = notifier;
    }

    public async Task ShipAsync(int orderId)
    {
        await _notifier.SendAsync($"Order {orderId} shipped");
    }
}
```

Production can use:

```csharp
var service = new OrderService(new ConsoleNotifier());
```

Tests can use:

```csharp
public sealed class FakeNotifier : INotifier
{
    public List<string> SentMessages { get; } = new();

    public Task SendAsync(string message)
    {
        SentMessages.Add(message);
        return Task.CompletedTask;
    }
}
```

Now the test does not send real email or depend on console output.

---

## Inheritance

Inheritance says:

```text
This class is a kind of another class.
```

```csharp
public abstract class Shape
{
    public abstract decimal Area();
}

public sealed class Rectangle : Shape
{
    public Rectangle(decimal width, decimal height)
    {
        Width = width;
        Height = height;
    }

    public decimal Width { get; }
    public decimal Height { get; }

    public override decimal Area()
    {
        return Width * Height;
    }
}
```

Use:

```csharp
Shape shape = new Rectangle(10, 5);
Console.WriteLine(shape.Area());
```

---

## Interface Versus Abstract Class

| Need | Usually choose |
|---|---|
| define capabilities across unrelated types | interface |
| share base behavior and state | abstract class |
| allow multiple contracts | interface |
| model one clear base family | abstract class |

C# classes can implement multiple interfaces, but inherit from only one class.

---

## Prefer Composition For Has-A

Composition:

```csharp
public sealed class EmailSender
{
    public Task SendAsync(string address, string message)
    {
        Console.WriteLine($"Sending to {address}: {message}");
        return Task.CompletedTask;
    }
}

public sealed class OrderService
{
    private readonly EmailSender _emailSender;

    public OrderService(EmailSender emailSender)
    {
        _emailSender = emailSender;
    }
}
```

This says:

```text
OrderService has an EmailSender.
```

Do not use inheritance when the relationship is really "has-a."

---

## Common Mistakes

### Mistake 1: Interface For Every Class

Interfaces are useful at boundaries and substitution points.

They are not mandatory for every single class.

### Mistake 2: Inheritance For Code Reuse Only

If you only want to reuse a helper method, composition may be cleaner.

### Mistake 3: Interfaces That Are Too Big

Prefer small contracts:

```csharp
public interface IOrderReader
{
    Order? Find(int id);
}
```

instead of one huge interface that does everything.

---

## Mini Project: Payment Processors

```csharp
public interface IPaymentProcessor
{
    Task<bool> ChargeAsync(decimal amount);
}

public sealed class FakePaymentProcessor : IPaymentProcessor
{
    public Task<bool> ChargeAsync(decimal amount)
    {
        return Task.FromResult(amount > 0);
    }
}

public sealed class CheckoutService
{
    private readonly IPaymentProcessor _paymentProcessor;

    public CheckoutService(IPaymentProcessor paymentProcessor)
    {
        _paymentProcessor = paymentProcessor;
    }

    public async Task<bool> CheckoutAsync(decimal total)
    {
        if (total <= 0)
        {
            return false;
        }

        return await _paymentProcessor.ChargeAsync(total);
    }
}
```

Explain:

- why `CheckoutService` depends on `IPaymentProcessor`
- why the fake processor is useful in tests
- why payment behavior is injected instead of created inside the service

---

## Chapter Checkpoint

You should now be able to answer:

- What is an interface?
- What does it mean to implement an interface?
- What is polymorphism?
- Why are interfaces useful for tests?
- When might an abstract class be better than an interface?
- What is composition?
- Why should interfaces stay small?

---

## Recap

- Interfaces define contracts.
- Polymorphism lets one variable work with many implementations.
- Interfaces are great at boundaries and test seams.
- Inheritance models "is-a."
- Composition models "has-a."
- Prefer small, purposeful interfaces.

## Try It Yourself

Create `IReceiptWriter` with two implementations:

- `ConsoleReceiptWriter`
- `MemoryReceiptWriter`

Use both through an `IReceiptWriter` variable.

---

[**Next ->** Records, Structs, And When To Use Them](./04-records-structs-and-when-to-use-them.md)  
[**<- Previous** Constructors And Object Lifecycle](./02-constructors-and-object-lifecycle.md)
