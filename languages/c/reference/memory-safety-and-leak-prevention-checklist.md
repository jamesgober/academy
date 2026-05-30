# Memory Safety And Leak Prevention Checklist

[Home](../../../README.md) / [C](../README.md) / [Reference](./README.md)

---

> Use this checklist during C code review, debugging, and release preparation.

Related lessons:

- [Common Pointer Mistakes](../course/03-pointers-and-memory/05-common-pointer-mistakes.md)
- [Avoiding Leaks, Dangling Pointers, And Double Free](../course/03-pointers-and-memory/07-avoiding-leaks-dangling-pointers-and-double-free.md)
- [Safe String Handling Patterns](../course/04-structs-arrays-and-strings/04-safe-string-handling-patterns.md)

---

## Allocation And Cleanup

- Every `malloc` or `calloc` result is checked for `NULL`.
- Every owned allocation has exactly one intended `free`.
- Early returns still release owned resources.
- Cleanup order is obvious.
- Pointers that remain in scope are set to `NULL` after `free` where practical.
- API comments say who owns returned memory.

---

## Pointer Validity

- No uninitialized pointer is read or dereferenced.
- Nullable pointers are checked before dereference.
- No pointer outlives the object it points to.
- No address of a local variable is returned.
- No use-after-free.
- No double free.
- Pointer types match the pointed-to objects.

---

## Arrays And Strings

- Array functions receive a count.
- Destination string functions receive capacity.
- Loops use `i < count`, not `i <= count`.
- String buffers reserve room for `'\0'`.
- `snprintf` results are checked for truncation.
- `fgets` input is checked and newline handling is deliberate.
- Fixed-size struct fields use named constants for capacity.

---

## Build And Runtime Gates

Strict warning build:

```bash
cc -Wall -Wextra -Wpedantic -Werror -std=c17 main.c -o app
```

AddressSanitizer build when available:

```bash
cc -Wall -Wextra -Wpedantic -std=c17 -fsanitize=address -g main.c -o app
```

Run with representative inputs:

```bash
./app
```

---

## Bug-Class Prompts

| Bug | Question |
|---|---|
| Leak | Which allocation was not freed? |
| Dangling pointer | What object stopped existing? |
| Use-after-free | Where was memory used after cleanup? |
| Double free | Which path frees the same allocation twice? |
| Out-of-bounds | Which index or length is wrong? |
| Buffer overflow | Which write ignored destination capacity? |
| Truncation | Was shortened output detected? |

---

[Reference Index](./README.md) / [C](../README.md) / [Home](../../../README.md)
