<h1 align="center">
    <img width="99" alt="C logo" src="../../../../_assets/logos/c.svg">
    <br>
    <b>C</b>
</h1>

<div align="center">

[Home](../../../../README.md) · [C](../../README.md) · [Chapter 03](./README.md)

</div>

---

# Chapter 03 Checkpoint

> Confirm you can reason about pointers and memory lifetime before moving on.

## Must-be-able checklist

- Explain value versus address clearly.
- Declare and use pointers with `&` and `*` correctly.
- Explain pass-by-value versus pass-by-address with a concrete example.
- Allocate heap memory with `malloc` or `calloc` and check for `NULL`.
- Free heap memory exactly once.
- Explain how to avoid leaks and dangling-pointer use.

## Practice task

Build a small "sensor buffer" example that:
- allocates an integer buffer on the heap
- fills and prints the values
- passes the buffer to one function for update
- frees memory and resets pointer to `NULL`

## Expected output characteristics

Your solution should:
- show data updated through pointer-based function logic
- include allocation-failure handling
- include explicit cleanup

## Reviewer checklist

- Is memory allocation checked for failure?
- Is ownership clear enough to identify who calls `free`?
- Is there any code path where allocated memory is leaked?
- Is the pointer value used after `free`?

---

## Next

Continue to [Chapter 04 — Structs, Arrays, and Strings](../04-structs-arrays-and-strings/README.md).

---

<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Avoiding Leaks, Dangling Pointers, and Double Free](./07-avoiding-leaks-dangling-pointers-and-double-free.md) | [Chapter 03](./README.md) · [C](../../README.md) · [Home](../../../../README.md) | [Chapter 04 →](../04-structs-arrays-and-strings/README.md) |

</div>
