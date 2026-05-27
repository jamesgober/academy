<h1 align="center">
    <img width="99" alt="C logo" src="../../../../_assets/logos/c.svg">
    <br>
    <b>C</b>
</h1>

<div align="center">

[Home](../../../../README.md) · [C](../../README.md) · [Chapter 03](./README.md)

</div>

---

# Common Pointer Mistakes

> Most pointer pain comes from a few repeated mistakes, not from some mysterious advanced curse.

**You will learn:**
- What beginners usually get wrong with pointers
- How to recognize invalid or uninitialized pointer usage
- Why careful step-by-step reasoning matters

**Before this page, you should know:** [Function Arguments and Pointers](./04-function-arguments-and-pointers.md)

---

## Common mistakes

- using an uninitialized pointer
- dereferencing a pointer that does not point to valid data
- confusing the pointer with the value it points to
- forgetting whether a function expects a value or an address

## Dangerous example

```c
int *ptr;
*ptr = 5;
```

Why this is bad:
- `ptr` has not been set to a valid address
- dereferencing it is undefined behavior

## Beginner-safe habits

- initialize variables before use
- reason about where the pointer came from
- ask "what address is stored here right now?"
- prefer small examples until the model feels stable

> [!WARNING]
> Pointer bugs are often memory bugs, and memory bugs can be hard to notice immediately.

---

## Recap

- Pointer mistakes usually come from invalid addresses or confused mental models.
- A pointer must point somewhere valid before dereferencing.
- Slow reasoning beats guessing.

## Try it yourself

Look at one pointer example and explain, line by line, which values are plain data and which values are addresses.

---

<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Function Arguments and Pointers](./04-function-arguments-and-pointers.md) | [Chapter 03](./README.md) · [C](../../README.md) · [Home](../../../../README.md) | [Dynamic Memory: `malloc`, `calloc`, `free` →](./06-dynamic-memory-malloc-calloc-free.md) |

</div>
