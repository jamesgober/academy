<h1 align="center">
    <img width="99" alt="C logo" src="../../../../_assets/logos/c.svg">
    <br>
    <b>C</b>
</h1>

<div align="center">

[Home](../../../../README.md) · [C](../../README.md) · [Chapter 04](./README.md)

</div>

---

# Structs and Grouped Data

> A struct lets you group related values into one custom data shape.

**You will learn:**
- What a struct is
- How struct fields are declared
- Why grouped data improves readability

**Before this page, you should know:** [Chapter 03](../03-pointers-and-memory/README.md)

---

## Basic struct

```c
struct Car {
    int speed;
    int fuel;
};
```

This defines a custom type with two fields.

## Using it

```c
struct Car c;
c.speed = 120;
c.fuel = 80;
```

## Why structs matter

Without structs, related values often become scattered variables.
Structs keep related state together.

---

## Recap

- Structs define grouped data.
- Fields are accessed with `.`.
- Grouping improves clarity and maintainability.

## Try it yourself

Define a `struct Player` with three fields and print them.

---

<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Chapter 03](../03-pointers-and-memory/README.md) | [Chapter 04](./README.md) · [C](../../README.md) · [Home](../../../../README.md) | [Arrays and Indexed Access →](./02-arrays-and-indexed-access.md) |

</div>
