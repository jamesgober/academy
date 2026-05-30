<h1 align="center">
    <img width="99" alt="C logo" src="../../../../_assets/logos/c.svg">
    <br>
    <b>C</b>
</h1>

[Home](../../../../README.md) / [C](../../README.md) / [Chapter 03](./README.md)

---

# What A Pointer Really Stores

> A pointer stores an address. Not the value at that address. The address.

**You will learn:**
- What a pointer variable is
- How to declare pointer variables
- Why pointer type matters
- How `NULL` works as "points to nothing"
- How to read pointer declarations without panic

**Before this page, you should know:** [Memory Addresses In Plain Language](./01-memory-addresses-in-plain-language.md)

---

## Pointer In One Sentence

A pointer is a variable whose value is a memory address.

```c
int speed = 120;
int *speed_ptr = &speed;
```

Now there are two variables:

```text
speed
  stores: 120

speed_ptr
  stores: address of speed
```

Visual model:

```text
speed_ptr
  |
  | stores address 0x1004
  v
speed at 0x1004
  |
  v
value 120
```

---

## Declare A Pointer

```c
int *speed_ptr;
```

Read it as:

```text
speed_ptr is a pointer to int
```

Then assign an address:

```c
speed_ptr = &speed;
```

Usually you do both at once:

```c
int *speed_ptr = &speed;
```

The type matters:

```c
double temperature = 98.6;
double *temperature_ptr = &temperature;
```

Read:

```text
temperature_ptr is a pointer to double
```

---

## Pointer Type Means "What Lives There"

An address is a location.

The pointer type tells C what kind of value to expect at that location.

```text
int *      -> address where an int lives
double *   -> address where a double lives
char *     -> address where a char lives
```

Why this matters:

- C needs to know how many bytes to read
- C needs to know how pointer arithmetic should move
- C needs to know which type rules apply

Pointer type is not decoration. It is part of the meaning.

---

## Print Pointer Values

```c
#include <stdio.h>

int main(void) {
    int speed = 120;
    int *speed_ptr = &speed;

    printf("speed value: %d\n", speed);
    printf("speed address: %p\n", (void *)&speed);
    printf("pointer value: %p\n", (void *)speed_ptr);

    return 0;
}
```

The last two lines should print the same address:

```text
speed address: 0x...
pointer value: 0x...
```

Because:

```text
speed_ptr stores &speed
```

---

## Declaration Style

You will see both:

```c
int* speed_ptr;
```

and:

```c
int *speed_ptr;
```

This course uses:

```c
int *speed_ptr;
```

Why?

Because this can surprise beginners:

```c
int* a, b;
```

Only `a` is a pointer.

`b` is an `int`.

Clearer:

```c
int *a;
int b;
```

Beginner rule:

> Declare one pointer variable per line until pointer declarations feel boring.

---

## `NULL`: A Pointer To Nothing

A pointer can intentionally point to nothing:

```c
int *speed_ptr = NULL;
```

`NULL` means:

```text
This pointer currently does not point to a valid object.
```

Before using a pointer, check if it is `NULL` when null is possible:

```c
if (speed_ptr != NULL) {
    printf("%d\n", *speed_ptr);
}
```

Never dereference `NULL`:

```c
int *ptr = NULL;
printf("%d\n", *ptr); // bug
```

That usually crashes.

---

## Uninitialized Pointers Are Dangerous

Bad:

```c
int *ptr;
printf("%p\n", (void *)ptr);
```

`ptr` has an indeterminate value. It does not automatically point to something
safe.

Safer:

```c
int *ptr = NULL;
```

or:

```c
int value = 10;
int *ptr = &value;
```

Beginner rule:

> Every pointer should either point to a real object or be `NULL`.

---

## Pointer To Const

Sometimes a function should read through a pointer but not modify the value.

```c
void print_score(const int *score_ptr) {
    if (score_ptr == NULL) {
        return;
    }

    printf("%d\n", *score_ptr);
}
```

`const int *` means:

```text
pointer to int that this function promises not to change through this pointer
```

This is useful for safer APIs.

---

## Common Pointer Sentences

Practice translating:

| Code | Plain English |
|---|---|
| `int value;` | `value` is an int |
| `int *ptr;` | `ptr` is a pointer to int |
| `ptr = &value;` | store the address of `value` in `ptr` |
| `ptr == NULL` | pointer points to nothing |
| `const int *ptr` | pointer to int that should not be changed through `ptr` |

---

## Mini Project: Pointer Description Program

```c
#include <stdio.h>

int main(void) {
    int score = 42;
    int *score_ptr = &score;
    int *missing_ptr = NULL;

    printf("score value: %d\n", score);
    printf("score address: %p\n", (void *)&score);
    printf("score_ptr stores: %p\n", (void *)score_ptr);
    printf("missing_ptr stores: %p\n", (void *)missing_ptr);

    return 0;
}
```

Write comments above each line explaining whether it prints:

- a normal integer value
- an address
- a null pointer value

---

## Chapter Checkpoint

You should now be able to answer:

- What does a pointer store?
- What does `int *ptr` mean?
- Why does pointer type matter?
- Why is `int* a, b;` surprising?
- What does `NULL` mean?
- Why are uninitialized pointers dangerous?
- What does `const int *ptr` promise?

---

## Recap

- A pointer stores an address.
- `int *ptr` means pointer to int.
- `&value` gives an address that can be stored in a pointer.
- `NULL` means the pointer points to nothing.
- Uninitialized pointers are unsafe.
- Use one pointer declaration per line while learning.

## Try It Yourself

Create:

- an `int`
- a `double`
- a pointer to each
- one `NULL` pointer

Print the values and pointer values, then explain what each pointer stores.

---

[**Next ->** Dereferencing Without Hand-Waving](./03-dereferencing-without-hand-waving.md)  
[**<- Previous** Memory Addresses In Plain Language](./01-memory-addresses-in-plain-language.md)
