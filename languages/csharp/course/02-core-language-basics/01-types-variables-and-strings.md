<h1 align="center">
    <img width="99" alt="C# logo" src="../../../../_assets/logos/cs.svg">
    <br>
    <b>C#</b>
</h1>

<!-- ===== HEAD NAV ===== -->
<div align="center">

[Home](../../../../README.md) · [C#](../../README.md) · [Chapter 02](./README.md)

</div>

---

# Types, Variables, and Strings

> Reliable software starts with choosing the right type for the data you store.

**You will learn:**
- Core C# built-in types and when to use them
- Variable declaration with `var` and explicit types
- String basics and interpolation

**Before this page, you should know:** Chapter 01 command flow.

---

## Core type map

- `int` for general whole numbers
- `long` for larger whole numbers
- `double` for decimal math where tiny precision drift is acceptable
- `decimal` for money and high precision decimal values
- `bool` for true/false state
- `string` for text

## `var` versus explicit type

Use `var` when the right side is obvious.

```csharp
var count = 3;          // int
string title = "Order";
```

If intent is not obvious, prefer explicit type for readability.

## String interpolation

```csharp
string name = "Ava";
int level = 2;
string msg = $"{name} reached level {level}.";
```

Interpolation is clearer than complex string concatenation.

---

## Recap

- Pick types intentionally, especially for numbers.
- `var` is fine when type is obvious.
- Use interpolation for readable text output.

## Try it yourself

Create variables using `int`, `decimal`, and `string`, then print them in one interpolated message.

---

<!-- ===== FOOT NAV ===== -->
<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Chapter Start](./README.md) | [Chapter](./README.md) · [Track](../../README.md) · [Home](../../../../README.md) | [Methods, Parameters, and Returns →](./02-methods-parameters-and-returns.md) |

</div>
