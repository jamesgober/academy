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

# Records, Structs, and When to Use Them

> Choosing class, record, or struct changes equality, mutability, and performance behavior.

**You will learn:**
- When to use class, record, and struct
- Value equality with records
- Why mutable structs are dangerous

**Before this page, you should know:** interface and class design basics.

---

## Type choices

- `class`: reference type, best default for rich domain behavior
- `record`: reference type with value-based equality semantics
- `struct`: value type, best for small immutable data

```csharp
public record Money(string Currency, decimal Amount);

public readonly struct Point
{
    public int X { get; }
    public int Y { get; }
    public Point(int x, int y) => (X, Y) = (x, y);
}
```

Guideline:
- prefer immutable records/structs
- avoid mutable structs in beginner projects

---

## Recap

- Class is the default for complex behavior.
- Record is excellent for immutable data models.
- Struct is for small, value-like data.

## Try it yourself

Model an `Address` as a record and compare equality of two instances with same values.

---

<!-- ===== FOOT NAV ===== -->
<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Interfaces, Inheritance, and Polymorphism](./03-interfaces-inheritance-and-polymorphism.md) | [Chapter](./README.md) · [Track](../../README.md) · [Home](../../../../README.md) | [Chapter 03 Checkpoint →](./05-chapter-03-checkpoint.md) |

</div>
