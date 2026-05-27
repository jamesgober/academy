# Pointers and Dynamic Memory Cheat Sheet

Quick lookup for address and heap-memory basics.

## Pointer basics

```c
int speed = 120;
int *ptr = &speed;
```

- `&speed` gives address
- `ptr` stores address
- `*ptr` reads/writes value at that address

## Dynamic allocation

```c
int *arr = malloc(5 * sizeof(int));
if (arr == NULL) {
    return 1;
}

free(arr);
arr = NULL;
```

## `malloc` vs `calloc`

- `malloc(n)` allocates raw bytes
- `calloc(count, size)` allocates and zero-initializes

## Ownership prompt

For every allocation, answer:
- who owns it?
- who frees it?
- what happens on early-return paths?

---

[← Reference Index](./README.md) · [C](../README.md)
