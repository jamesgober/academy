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

# Interfaces, Inheritance, and Polymorphism

> Program to contracts so code stays replaceable and testable.

**You will learn:**
- How interfaces define behavior contracts
- Where inheritance helps and where it hurts
- Runtime polymorphism through interface references

**Before this page, you should know:** class and constructor basics.

---

## Interface-first design

```csharp
public interface INotifier
{
    Task SendAsync(string message);
}

public class EmailNotifier : INotifier
{
    public Task SendAsync(string message)
    {
        Console.WriteLine($"Email: {message}");
        return Task.CompletedTask;
    }
}
```

Usage:

```csharp
INotifier notifier = new EmailNotifier();
await notifier.SendAsync("Order shipped");
```

## Inheritance guidance

Use inheritance for true "is-a" relationships. Prefer composition when behavior is optional or likely to vary.

---

## Recap

- Interfaces decouple callers from implementations.
- Polymorphism allows runtime substitution.
- Inheritance is useful but should be used conservatively.

## Try it yourself

Create `IPaymentProcessor` with two implementations and call them via interface references.

---

<!-- ===== FOOT NAV ===== -->
<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Constructors and Object Lifecycle](./02-constructors-and-object-lifecycle.md) | [Chapter](./README.md) · [Track](../../README.md) · [Home](../../../../README.md) | [Records, Structs, and When to Use Them →](./04-records-structs-and-when-to-use-them.md) |

</div>
