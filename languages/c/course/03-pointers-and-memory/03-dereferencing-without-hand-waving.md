<h1 align="center">
    <img width="99" alt="C logo" src="../../../../_assets/logos/c.svg">
    <br>
    <b>C</b>
</h1>

[Home](../../../../README.md) / [C](../../README.md) / [Chapter 03](./README.md)

---

# Dereferencing Without Hand-Waving

> Dereferencing means: take the address stored in a pointer, go there, and use
> the value at that location.

**You will learn:**
- What dereferencing does
- Why `*` has different meanings in declarations and expressions
- How to read through a pointer
- How to write through a pointer
- Why null and invalid pointers are dangerous
- How `const` affects dereferencing

**Before this page, you should know:** [What A Pointer Really Stores](./02-what-a-pointer-really-stores.md)

---

## The Core Move

```c
int speed = 120;
int *speed_ptr = &speed;
```

Picture:

```text
speed_ptr stores address of speed
        |
        v
speed stores 120
```

Dereference:

```c
*speed_ptr
```

Read it as:

```text
the int value at the address stored in speed_ptr
```

---

## Read Through A Pointer

```c
#include <stdio.h>

int main(void) {
    int speed = 120;
    int *speed_ptr = &speed;

    printf("%d\n", *speed_ptr);

    return 0;
}
```

Output:

```text
120
```

Step by step:

```text
speed_ptr contains an address.
*speed_ptr follows that address.
C reads the int stored there.
printf prints that int.
```

---

## Write Through A Pointer

```c
int speed = 120;
int *speed_ptr = &speed;

*speed_ptr = 150;

printf("%d\n", speed);
```

Output:

```text
150
```

Why?

`speed_ptr` points at `speed`. Writing through the pointer writes to the same
memory box that `speed` names.

Visual model:

```text
Before:

speed_ptr -> speed -> 120

After *speed_ptr = 150:

speed_ptr -> speed -> 150
```

---

## Two Meanings Of `*`

The same symbol appears in two pointer contexts.

Declaration:

```c
int *ptr;
```

Means:

```text
ptr is a pointer to int
```

Expression:

```c
*ptr
```

Means:

```text
the int value at the address stored in ptr
```

Table:

| Code | Context | Meaning |
|---|---|---|
| `int *ptr` | Declaration | Create a pointer variable |
| `*ptr` | Expression | Dereference pointer |
| `*ptr = 5` | Expression assignment | Write through pointer |

Context decides the meaning.

---

## Null Check Before Dereference

If a pointer can be `NULL`, check it before dereferencing.

```c
void print_score(const int *score_ptr) {
    if (score_ptr == NULL) {
        printf("no score\n");
        return;
    }

    printf("score: %d\n", *score_ptr);
}
```

This is safe:

```c
int score = 90;
print_score(&score);
print_score(NULL);
```

This is not:

```c
int *score_ptr = NULL;
printf("%d\n", *score_ptr);
```

Dereferencing `NULL` is undefined behavior. In practice, it often crashes.

---

## Undefined Behavior In Plain Language

Undefined behavior means:

```text
The C language no longer promises what your program does.
```

It might:

- Crash immediately
- Print nonsense
- Seem to work
- Break later
- Behave differently after compiler optimization

Invalid pointer dereferences are a major source of undefined behavior.

Beginner rule:

> Never dereference a pointer unless you know it points to a valid object of the
> correct type.

---

## Dereferencing With `const`

```c
void print_value(const int *value_ptr) {
    if (value_ptr == NULL) {
        return;
    }

    printf("%d\n", *value_ptr);
}
```

Inside this function, this is allowed:

```c
printf("%d\n", *value_ptr);
```

This is rejected:

```c
*value_ptr = 99;
```

`const int *` means:

```text
You may read the int through this pointer.
You may not modify the int through this pointer.
```

Use `const` for pointer parameters that should only read.

---

## Pointer To Pointer Preview

You may eventually see:

```c
int **ptr_to_ptr;
```

That means:

```text
pointer to pointer to int
```

Do not rush there. The same model still applies:

```text
pointer stores an address
dereference follows one level
```

For now, master one level:

```c
int *ptr;
*ptr;
```

---

## Mini Project: Read And Write Through A Pointer

```c
#include <stdio.h>

int main(void) {
    int score = 10;
    int *score_ptr = &score;

    printf("before: %d\n", score);
    printf("through pointer: %d\n", *score_ptr);

    *score_ptr = 25;

    printf("after: %d\n", score);

    return 0;
}
```

Expected:

```text
before: 10
through pointer: 10
after: 25
```

Explain each print line in terms of:

```text
direct variable access
pointer dereference read
pointer dereference write
```

---

## Debugging Questions

When dereferencing crashes, ask:

```text
Is the pointer NULL?
Was the pointed-to variable still alive?
Was the memory freed?
Is the pointer the correct type?
Was the pointer initialized?
Did an array index go out of bounds?
```

You will revisit these with sanitizers later.

---

## Chapter Checkpoint

You should now be able to answer:

- What does dereferencing mean?
- What does `*ptr` mean in an expression?
- How is `int *ptr` different from `*ptr`?
- What happens when you write `*ptr = 5`?
- Why should nullable pointers be checked?
- What does `const int *ptr` prevent?
- What is undefined behavior?

---

## Recap

- Dereferencing follows a pointer to the value it points at.
- `*ptr` can read.
- `*ptr = value` can write.
- The `*` in a declaration and the `*` in an expression have different roles.
- Never dereference `NULL` or invalid pointers.
- Use `const` when a function should read but not modify.

## Try It Yourself

Write a program that:

1. Creates an `int`.
2. Points at it.
3. Prints the value directly.
4. Prints the value through the pointer.
5. Changes the value through the pointer.
6. Prints the original variable again.

---

[**Next ->** Function Arguments And Pointers](./04-function-arguments-and-pointers.md)  
[**<- Previous** What A Pointer Really Stores](./02-what-a-pointer-really-stores.md)
