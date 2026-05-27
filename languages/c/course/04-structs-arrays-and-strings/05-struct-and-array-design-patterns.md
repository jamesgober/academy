<h1 align="center">
    <img width="99" alt="C logo" src="../../../../_assets/logos/c.svg">
    <br>
    <b>C</b>
</h1>

<div align="center">

[Home](../../../../README.md) · [C](../../README.md) · [Chapter 04](./README.md)

</div>

---

# Struct and Array Design Patterns

> Clean data layout decisions make C code easier to reason about and safer to maintain.

**You will learn:**
- Common struct and array composition approaches
- How to pass structured data safely to functions
- How data-shape decisions influence memory safety

**Before this page, you should know:** [Safe String Handling Patterns](./04-safe-string-handling-patterns.md)

---

## Pattern 1: struct with fixed-size fields

```c
struct Car {
    char name[16];
    int speed;
};
```

Use when bounds are clear and fixed-size tradeoffs are acceptable.

## Pattern 2: array of structs

```c
struct Car garage[10];
```

Useful for small fixed-capacity collections.

## Design checks

- Are size limits explicit?
- Are boundaries validated before writes?
- Is ownership of heap allocations clear when pointers are involved?

> [!TIP]
> A simple fixed-size design is often safer for beginners than dynamic shape changes everywhere.

---

## Recap

- Struct and array layouts are architecture decisions, not random syntax choices.
- Explicit bounds reduce memory errors.
- Simpler layouts are usually easier to audit.

## Try it yourself

Design a struct-based inventory entry and an array that stores 20 entries. Write down one boundary rule for string fields.

---

<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Safe String Handling Patterns](./04-safe-string-handling-patterns.md) | [Chapter 04](./README.md) · [C](../../README.md) · [Home](../../../../README.md) | [Chapter 04 Checkpoint →](./06-chapter-04-checkpoint.md) |

</div>
