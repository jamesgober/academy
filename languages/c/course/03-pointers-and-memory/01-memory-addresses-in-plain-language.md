<h1 align="center">
    <img width="99" alt="C logo" src="../../../../_assets/logos/c.svg">
    <br>
    <b>C</b>
</h1>

[Home](../../../../README.md) / [C](../../README.md) / [Chapter 03](./README.md)

---

# Memory Addresses In Plain Language

> Before pointers make sense, memory addresses have to stop sounding magical.
> A memory address is just where a value lives.

**You will learn:**
- What memory is in a C program
- What a memory address represents
- How values and addresses are different
- How the `&` address-of operator works
- Why addresses matter more in C than in many beginner languages

**Before this page, you should know:** [Chapter 02](../02-syntax-and-control-flow/README.md)

---

## Memory As Numbered Boxes

Imagine your program's memory as a huge row of numbered boxes.

```text
Address      Value
---------    -----
0x1000       ?
0x1004       ?
0x1008       ?
0x100C       ?
```

When you create a variable, C stores the value in one or more boxes.

```c
int speed = 120;
```

Mental model:

```text
Variable name: speed
Address:       0x1004
Value:         120
```

The exact address is chosen by the program, compiler, operating system, and
runtime environment. You do not choose it in normal beginner code.

---

## Name, Value, Address

These are three different ideas:

| Idea | Example | Meaning |
|---|---|---|
| Variable name | `speed` | Human-friendly label in your code |
| Value | `120` | The data stored |
| Address | `0x1004` | Where the data lives in memory |

Visual model:

```text
speed
  |
  v
address 0x1004
  |
  v
value 120
```

Beginner mistake:

```text
"speed is 120, so the address is 120."
```

No. The value and address are separate.

---

## Print A Value

```c
#include <stdio.h>

int main(void) {
    int speed = 120;

    printf("value: %d\n", speed);
    return 0;
}
```

Output:

```text
value: 120
```

`%d` prints an `int` value.

---

## Print An Address

Use `&` to ask for a variable's address.

```c
#include <stdio.h>

int main(void) {
    int speed = 120;

    printf("value: %d\n", speed);
    printf("address: %p\n", (void *)&speed);

    return 0;
}
```

Important pieces:

| Code | Meaning |
|---|---|
| `speed` | The value stored in the variable |
| `&speed` | The address of the variable |
| `%p` | `printf` format for pointer/address output |
| `(void *)&speed` | Cast expected by `%p` |

The printed address might look like:

```text
address: 000000D7A8CFFB54
```

or:

```text
address: 0x7ffcc2b04b2c
```

Do not memorize the address. It can change every time the program runs.

---

## Why The Cast To `void *`?

For `printf("%p", ...)`, C expects a `void *`.

So this is the careful form:

```c
printf("%p\n", (void *)&speed);
```

Beginner translation:

```text
Print this address using the generic pointer type that printf expects.
```

Do not worry about `void *` deeply yet. You will see it again with memory
allocation.

---

## Addresses Can Be Different On Every Run

Run the program twice.

You may see different addresses.

That is normal.

Modern operating systems often randomize where programs live in memory. This is
part of normal security behavior.

What should stay true:

```text
speed has a value.
speed has an address.
&speed gives the address.
```

The exact number is not the lesson.

---

## Why C Lets You See Addresses

C is used for:

- Operating systems
- Embedded devices
- Drivers
- Game engines
- Compilers
- Performance-sensitive libraries
- Interfacing with hardware and other languages

Those jobs often need careful control over memory.

That power is why C is useful.

That power is also why C is dangerous.

In C, a wrong address can mean:

- Crash
- Corrupted data
- Security bug
- Program that seems to work until it doesn't

So we learn the model slowly.

---

## Address-Of Operator: `&`

The address-of operator goes before a variable:

```c
&speed
```

Read it as:

```text
the address of speed
```

Example:

```c
int count = 5;
printf("count value: %d\n", count);
printf("count address: %p\n", (void *)&count);
```

Do not confuse `&` here with logical AND in other contexts.

In C:

| Syntax | Meaning |
|---|---|
| `&x` | Address of `x` |
| `a & b` | Bitwise AND |
| `a && b` | Logical AND |

Same character, different context.

---

## Memory Safety Warning

Seeing an address does not mean you should randomly use it.

This is okay:

```c
printf("%p\n", (void *)&speed);
```

This is not beginner-safe:

```c
int *mystery = (int *)0x1004;
printf("%d\n", *mystery);
```

Do not invent addresses. Use addresses that come from real objects, allocation,
or APIs that document ownership.

---

## Mini Project: Value And Address Receipt

Write:

```c
#include <stdio.h>

int main(void) {
    int age = 30;
    double temperature = 98.6;
    char grade = 'A';

    printf("age value: %d\n", age);
    printf("age address: %p\n", (void *)&age);

    printf("temperature value: %.1f\n", temperature);
    printf("temperature address: %p\n", (void *)&temperature);

    printf("grade value: %c\n", grade);
    printf("grade address: %p\n", (void *)&grade);

    return 0;
}
```

Then answer:

```text
Which parts are values?
Which parts are addresses?
Did addresses look like ordinary numbers?
Did addresses change when you ran the program again?
```

---

## Chapter Checkpoint

You should now be able to answer:

- What is a memory address?
- What is the difference between a variable name, value, and address?
- What does `&speed` mean?
- Why does `%p` use `(void *)`?
- Why should you not invent random addresses?
- Why does C expose addresses at all?

---

## Recap

- Variables live somewhere in memory.
- A memory address is where a value lives.
- `speed` reads the value.
- `&speed` gives the address.
- `%p` prints an address-like value.
- Pointers build directly on this address model.

## Try It Yourself

Declare two integers with different values. Print each value and each address.
Then write a short explanation:

```text
The value is...
The address is...
They are different because...
```

---

[**Next ->** What A Pointer Really Stores](./02-what-a-pointer-really-stores.md)  
[**<- Previous** Chapter 02](../02-syntax-and-control-flow/README.md)
