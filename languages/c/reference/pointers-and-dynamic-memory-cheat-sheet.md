# Pointers And Dynamic Memory Reference

[Home](../../../README.md) / [C](../README.md) / [Reference](./README.md)

---

> Quick lookup for addresses, pointers, dereferencing, pointer parameters,
> `malloc`, `calloc`, `free`, and ownership.

Full lessons:

- [Memory Addresses In Plain Language](../course/03-pointers-and-memory/01-memory-addresses-in-plain-language.md)
- [What A Pointer Really Stores](../course/03-pointers-and-memory/02-what-a-pointer-really-stores.md)
- [Dynamic Memory: `malloc`, `calloc`, `free`](../course/03-pointers-and-memory/06-dynamic-memory-malloc-calloc-free.md)

---

## Address And Pointer Basics

| Syntax | Meaning |
|---|---|
| `value` | read variable value |
| `&value` | address of variable |
| `int *ptr` | pointer to int |
| `ptr = &value` | store address in pointer |
| `*ptr` | value at pointed-to address |
| `*ptr = 5` | write through pointer |
| `ptr == NULL` | pointer points to nothing |

Example:

```c
int speed = 120;
int *ptr = &speed;

printf("%p\n", (void *)ptr);
printf("%d\n", *ptr);
```

---

## Function Parameter Patterns

Read-only pointer:

```c
void print_score(const int *score);
```

Mutable pointer:

```c
bool add_bonus(int *score, int bonus);
```

Output parameter:

```c
bool average_scores(const int *scores, size_t count, double *average_out);
```

Array parameter:

```c
void print_scores(const int scores[], size_t count);
```

Rule:

> If a function receives an array or destination buffer, it should also receive
> the relevant count or capacity.

---

## Dynamic Allocation

```c
int *arr = malloc(count * sizeof *arr);
if (arr == NULL) {
    return 1;
}

/* use arr */

free(arr);
arr = NULL;
```

`calloc`:

```c
int *arr = calloc(count, sizeof *arr);
```

| Function | Use |
|---|---|
| `malloc(bytes)` | raw uninitialized storage |
| `calloc(count, size)` | zero-initialized storage |
| `free(ptr)` | release heap allocation |

---

## Ownership Prompts

For every heap pointer, answer:

```text
Who owns this memory?
Who frees it?
Can NULL happen?
Can an early return skip cleanup?
Can someone use it after free?
Can it be freed twice?
```

Good API comment:

```c
/*
 * Caller owns returned buffer and must free it.
 * Returns NULL on allocation failure.
 */
int *make_buffer(size_t count);
```

---

## Risk Notices

| Pattern | Risk |
|---|---|
| Uninitialized pointer | may point anywhere |
| Dereferencing `NULL` | undefined behavior |
| Returning `&local` | dangling pointer |
| Reading after `free` | use-after-free |
| Calling `free` twice | double free |
| Wrong `sizeof` | under-allocation |
| Missing array count | out-of-bounds access |

---

[Reference Index](./README.md) / [C](../README.md) / [Home](../../../README.md)
