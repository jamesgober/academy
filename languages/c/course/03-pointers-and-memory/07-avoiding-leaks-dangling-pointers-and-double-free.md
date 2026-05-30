<h1 align="center">
    <img width="99" alt="C logo" src="../../../../_assets/logos/c.svg">
    <br>
    <b>C</b>
</h1>

[Home](../../../../README.md) / [C](../../README.md) / [Chapter 03](./README.md)

---

# Avoiding Leaks, Dangling Pointers, And Double Free

> Most severe C memory bugs are lifetime bugs: memory is not freed, freed too
> early, or freed more than once.

**You will learn:**
- What memory leaks are
- What dangling pointers are
- What use-after-free means
- Why double free is dangerous
- How ownership rules prevent bugs
- How cleanup paths and sanitizers help

**Before this page, you should know:** [Dynamic Memory: `malloc`, `calloc`, `free`](./06-dynamic-memory-malloc-calloc-free.md)

---

## Memory Leak

A leak happens when allocated heap memory is never freed.

```c
void leak_example(void) {
    int *numbers = malloc(5 * sizeof *numbers);
    if (numbers == NULL) {
        return;
    }

    numbers[0] = 10;
    /* forgot free(numbers) */
}
```

When the function returns, the pointer variable is gone, but the heap allocation
is still reserved.

The program lost the address needed to free it.

Why leaks matter:

- Small short-lived program: maybe not noticeable
- Long-running service: memory grows over time
- Embedded system: limited memory can be exhausted
- Library code: caller may pay for your leak repeatedly

---

## Dangling Pointer

A dangling pointer points to memory that is no longer valid.

```c
int *numbers = malloc(5 * sizeof *numbers);
if (numbers == NULL) {
    return 1;
}

free(numbers);

printf("%d\n", numbers[0]); // use-after-free bug
```

After `free(numbers)`, the allocation is no longer yours.

The pointer still contains an address, but the address is not valid for use.

Set it to `NULL` after freeing when the variable remains in scope:

```c
free(numbers);
numbers = NULL;
```

This does not fix other copies of the pointer. It only clears this variable.

---

## Use-After-Free

Use-after-free means:

```text
free memory
then read or write through an old pointer
```

Bad:

```c
char *name = malloc(20);
if (name == NULL) {
    return 1;
}

free(name);
name[0] = 'A';
```

This is undefined behavior.

Use-after-free bugs can become security vulnerabilities because the memory may
have been reused for something else.

---

## Double Free

Double free means freeing the same allocation twice.

```c
int *numbers = malloc(5 * sizeof *numbers);
if (numbers == NULL) {
    return 1;
}

free(numbers);
free(numbers); // bug
```

This is undefined behavior.

It can corrupt the allocator's internal bookkeeping.

Safer local habit:

```c
free(numbers);
numbers = NULL;
free(numbers); // safe no-op, but still suspicious design
```

But do not use "freeing NULL is safe" as an excuse for unclear ownership. The
best fix is freeing exactly once by design.

---

## Ownership In C

C does not track ownership for you.

You must write and follow ownership rules.

Good API comments:

```c
/*
 * Returns a newly allocated buffer.
 * Caller owns the returned pointer and must free it.
 * Returns NULL on allocation failure.
 */
int *make_buffer(size_t count);
```

```c
/*
 * Borrows scores for the duration of the call.
 * Does not store the pointer.
 * Does not free scores.
 */
int total_scores(const int *scores, size_t count);
```

```c
/*
 * Takes ownership of buffer and frees it before returning.
 * Caller must not use buffer after this call.
 */
void consume_buffer(int *buffer);
```

Ownership words:

| Word | Meaning |
|---|---|
| owns | responsible for freeing |
| borrows | uses temporarily, does not free |
| transfers | ownership moves to another function |
| caller frees | caller owns cleanup |
| callee frees | function owns cleanup |

---

## One Clear Cleanup Path

Risky:

```c
int run(void) {
    int *a = malloc(10 * sizeof *a);
    if (a == NULL) {
        return 1;
    }

    int *b = malloc(20 * sizeof *b);
    if (b == NULL) {
        return 1; // leaks a
    }

    free(b);
    free(a);
    return 0;
}
```

Better:

```c
int run(void) {
    int status = 1;
    int *a = NULL;
    int *b = NULL;

    a = malloc(10 * sizeof *a);
    if (a == NULL) {
        goto cleanup;
    }

    b = malloc(20 * sizeof *b);
    if (b == NULL) {
        goto cleanup;
    }

    status = 0;

cleanup:
    free(b);
    free(a);
    return status;
}
```

This pattern works because:

```text
All pointers start as NULL.
free(NULL) is safe.
Every path goes through cleanup.
```

---

## Do Not Return Stack Addresses

Bad:

```c
int *make_score(void) {
    int score = 100;
    return &score;
}
```

`score` dies when the function returns.

Return by value:

```c
int make_score(void) {
    return 100;
}
```

Or allocate with documented ownership:

```c
int *make_score(void) {
    int *score = malloc(sizeof *score);
    if (score == NULL) {
        return NULL;
    }

    *score = 100;
    return score;
}
```

Caller must free:

```c
int *score = make_score();
if (score != NULL) {
    printf("%d\n", *score);
    free(score);
}
```

---

## Use Sanitizers

When available, compile with AddressSanitizer:

```bash
cc -Wall -Wextra -Wpedantic -std=c17 -fsanitize=address -g main.c -o main
```

Run:

```bash
./main
```

AddressSanitizer can catch:

- use-after-free
- out-of-bounds access
- some leaks
- invalid frees

MSVC has AddressSanitizer support too, but command flags differ.

Sanitizers do not replace understanding. They are a flashlight.

---

## Memory Bug Checklist

For every heap pointer:

```text
Where was it allocated?
Was allocation checked?
Who owns it?
Who frees it?
Can any path skip free?
Can any path free twice?
Can anyone use it after free?
Are there other pointer copies?
Is the size calculation correct?
```

For every borrowed pointer:

```text
Who owns the memory?
How long is it valid?
Can it be NULL?
Does the function store it anywhere?
```

---

## Mini Project: Safe Buffer Lifecycle

Write:

```c
int *make_buffer(size_t count);
void fill_buffer(int *buffer, size_t count);
void print_buffer(const int *buffer, size_t count);
void free_buffer(int **buffer_ptr);
```

`free_buffer` takes a pointer to pointer so it can set the caller's pointer to
`NULL`:

```c
void free_buffer(int **buffer_ptr) {
    if (buffer_ptr == NULL || *buffer_ptr == NULL) {
        return;
    }

    free(*buffer_ptr);
    *buffer_ptr = NULL;
}
```

Call:

```c
int *buffer = make_buffer(5);
if (buffer == NULL) {
    return 1;
}

fill_buffer(buffer, 5);
print_buffer(buffer, 5);
free_buffer(&buffer);
```

After `free_buffer(&buffer)`, `buffer == NULL`.

---

## Chapter Checkpoint

You should now be able to answer:

- What is a memory leak?
- What is a dangling pointer?
- What is use-after-free?
- Why is double free dangerous?
- Why does setting a pointer to `NULL` help only that variable?
- What does "caller owns returned pointer" mean?
- Why can one cleanup path reduce leaks?
- What can AddressSanitizer help find?

---

## Recap

- Leaks happen when allocated memory is never freed.
- Dangling pointers point to invalid memory.
- Use-after-free means using memory after `free`.
- Double free means freeing the same allocation twice.
- C ownership must be documented and followed by humans.
- Cleanup paths and sanitizers reduce memory bugs.

## Try It Yourself

Take `make_sequence` from the previous lesson and add:

- an ownership comment
- a caller that frees correctly
- an AddressSanitizer build command
- a deliberate use-after-free bug, then remove it after seeing the sanitizer
  report

---

[**Next ->** Chapter 03 Checkpoint](./08-chapter-03-checkpoint.md)  
[**<- Previous** Dynamic Memory: `malloc`, `calloc`, `free`](./06-dynamic-memory-malloc-calloc-free.md)
