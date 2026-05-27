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

# Classes, Fields, and Properties

> C# class design is about encapsulating state and behavior safely.

**You will learn:**
- Difference between fields and properties
- Why auto-properties are common in modern C#
- Basic encapsulation habits

**Before this page, you should know:** methods and return values.

---

## Class basics

```csharp
public class Product
{
    private decimal _price;

    public string Name { get; set; } = "";

    public decimal Price
    {
        get => _price;
        set => _price = value < 0 ? 0 : value;
    }
}
```

Guidance:
- Keep fields private unless there is a strong reason not to.
- Expose controlled access through properties.
- Put validation where state can change.

## Why properties over public fields

Properties allow validation, change notifications, or logging later without breaking consumers.

---

## Recap

- Class fields hold private state.
- Properties expose safe access to that state.
- Validation near state mutation prevents invalid objects.

## Try it yourself

Create a `UserProfile` class with two validated properties.

---

<!-- ===== FOOT NAV ===== -->
<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Chapter Start](./README.md) | [Chapter](./README.md) · [Track](../../README.md) · [Home](../../../../README.md) | [Constructors and Object Lifecycle →](./02-constructors-and-object-lifecycle.md) |

</div>
