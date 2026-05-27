<h1 align="center">
    <img width="99" alt="C logo" src="../../../../_assets/logos/c.svg">
    <br>
    <b>C</b>
</h1>

<div align="center">

[Home](../../../../README.md) · [C](../../README.md) · [Chapter 03](./README.md)

</div>

---

# Avoiding Leaks, Dangling Pointers, and Double Free

> Most severe C memory bugs are lifecycle bugs: memory is used after lifetime is wrong.

**You will learn:**
- What memory leaks are
- What dangling pointers are
- Why double-free is dangerous
- Practical rules that prevent these bugs

**Before this page, you should know:** [Dynamic Memory: `malloc`, `calloc`, `free`](./06-dynamic-memory-malloc-calloc-free.md)

---

## Memory leak

A leak happens when heap memory is allocated but never freed.
Over time this wastes memory and can crash long-running programs.

## Dangling pointer

A dangling pointer points to memory that is no longer valid.
Example: free memory, then keep using the old pointer.

## Double free

Double free means calling `free` on the same allocation twice.
This is undefined behavior and can corrupt program state.

## Safer patterns

- Pair each allocation with one clear cleanup path.
- After `free(ptr)`, set `ptr = NULL`.
- Avoid sharing ownership of one heap allocation across many unrelated places.
- Use early returns carefully so cleanup still runs.

## Cleanup pattern sketch

```c
int *data = malloc(size);
if (data == NULL) {
    return 1;
}

/* use data */

free(data);
data = NULL;
```

> [!IMPORTANT]
> "Who owns this memory and who frees it?" should be answerable for every heap allocation.

---

## Recap

- Leaks waste memory by skipping `free`.
- Dangling pointers use invalid memory.
- Double-free releases the same memory twice.
- Explicit ownership and cleanup paths prevent most beginner memory bugs.

## Try it yourself

Take one function that allocates memory and write down the ownership rule in one sentence.

---

<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Dynamic Memory: `malloc`, `calloc`, `free`](./06-dynamic-memory-malloc-calloc-free.md) | [Chapter 03](./README.md) · [C](../../README.md) · [Home](../../../../README.md) | [Chapter 03 Checkpoint →](./08-chapter-03-checkpoint.md) |

</div>
