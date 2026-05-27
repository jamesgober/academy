<h1 align="center">
    <img width="99" alt="C logo" src="../../../../_assets/logos/c.svg">
    <br>
    <b>C</b>
</h1>

<div align="center">

[Home](../../../../README.md) · [C](../../README.md) · [Chapter 03](./README.md)

</div>

---

# Dynamic Memory: `malloc`, `calloc`, `free`

> Heap memory gives flexibility, but you must manage it manually in C.

**You will learn:**
- What dynamic allocation means
- When to use `malloc` versus `calloc`
- Why `free` is required to avoid leaks

**Before this page, you should know:** [Common Pointer Mistakes](./05-common-pointer-mistakes.md)

---

## Stack versus heap (beginner model)

- stack: automatic lifetime managed by scope
- heap: manual lifetime managed by your code

Dynamic memory lives on the heap.

## `malloc` example

```c
int *numbers = malloc(3 * sizeof(int));
if (numbers == NULL) {
    return 1;
}
```

## `calloc` example

```c
int *numbers = calloc(3, sizeof(int));
if (numbers == NULL) {
    return 1;
}
```

`calloc` also allocates memory, but initializes bytes to zero.

## `free`

```c
free(numbers);
numbers = NULL;
```

If you allocate memory and do not free it, you leak memory.

> [!IMPORTANT]
> Allocation and cleanup should be designed together. If you write `malloc`,
> decide where `free` happens before you move on.

---

## Recap

- `malloc` allocates raw heap memory.
- `calloc` allocates and zero-initializes.
- `free` releases heap memory and prevents leaks.

## Try it yourself

Allocate an array of 5 integers on the heap, fill it in a loop, print the
values, then free it.

---

<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Common Pointer Mistakes](./05-common-pointer-mistakes.md) | [Chapter 03](./README.md) · [C](../../README.md) · [Home](../../../../README.md) | [Avoiding Leaks, Dangling Pointers, and Double Free →](./07-avoiding-leaks-dangling-pointers-and-double-free.md) |

</div>
