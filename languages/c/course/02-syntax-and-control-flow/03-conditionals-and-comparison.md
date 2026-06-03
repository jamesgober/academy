<h1 align="center">
    <img width="99" alt="C logo" src="../../../../_assets/logos/c.svg">
    <br>
    <b>C</b>
</h1>

[Home](../../../../README.md) / [C](../../README.md) / [Chapter 02](./README.md)

---

# Conditionals And Comparison

Programs become useful when they can choose what to do.

```text
if the garage is full -> reject another car
otherwise             -> accept the car
```

C expresses decisions with conditions.

## Basic `if`

```c
if (age >= 18) {
    printf("adult\n");
}
```

The block runs only when the condition is true.

```text
age = 20
20 >= 18 is true
print adult
```

```text
age = 15
15 >= 18 is false
skip the block
```

## `if` And `else`

```c
if (score >= 50) {
    printf("pass\n");
} else {
    printf("fail\n");
}
```

`else` handles the other path.

```text
condition true  -> run if block
condition false -> run else block
```

## Common Comparison Operators

```text
==  equal to
!=  not equal to
>   greater than
<   less than
>=  greater than or equal to
<=  less than or equal to
```

Examples:

```c
if (temperature > 20) {
    printf("warm\n");
}

if (cars_in_garage == capacity) {
    printf("full\n");
}

if (attempts != 0) {
    printf("attempts used\n");
}
```

## Assignment Versus Comparison

This assigns:

```c
cars = 10;
```

This compares:

```c
cars == 10
```

Mixing them up is one of the classic C mistakes.

Bad:

```c
if (cars = 10) {
    printf("cars is now 10\n");
}
```

That code assigns `10` to `cars`. It does not check whether `cars` was already
10.

Good:

```c
if (cars == 10) {
    printf("cars is 10\n");
}
```

## `else if`

Use `else if` when there are several related choices.

```c
if (score >= 90) {
    printf("A\n");
} else if (score >= 80) {
    printf("B\n");
} else if (score >= 70) {
    printf("C\n");
} else {
    printf("Needs practice\n");
}
```

Order matters. The first true branch runs, then the rest are skipped.

## Combining Conditions

C uses logical operators:

```text
&&  and
||  or
!   not
```

Example:

```c
if (age >= 16 && has_permit) {
    printf("can practice driving\n");
}
```

Both sides must be true.

Example:

```c
if (role == 1 || role == 2) {
    printf("staff access\n");
}
```

At least one side must be true.

Example:

```c
if (!is_open) {
    printf("closed\n");
}
```

`!` flips true to false and false to true.

## Realistic Example: Garage Status

```c
#include <stdio.h>

void print_garage_status(int cars, int capacity) {
    if (cars < 0 || capacity <= 0) {
        printf("invalid garage data\n");
    } else if (cars == 0) {
        printf("garage is empty\n");
    } else if (cars >= capacity) {
        printf("garage is full\n");
    } else {
        printf("spaces available\n");
    }
}

int main(void) {
    print_garage_status(3, 10);
    print_garage_status(10, 10);
    print_garage_status(0, 10);

    return 0;
}
```

This example checks invalid data first. That is a useful pattern: handle bad or
special cases before the normal case.

## Common Beginner Mistakes

### Forgetting Braces

C allows single-statement `if` blocks without braces, but beginners should use
braces every time:

```c
if (score >= 50) {
    printf("pass\n");
}
```

Braces make future edits safer.

### Comparing Floating-Point Values Exactly

Avoid relying on exact equality for `double` results that came from math:

```c
if (average == 0.3) {
    printf("exact\n");
}
```

Floating-point numbers can contain tiny rounding differences. Prefer range
checks when decimals are involved.

### Writing Conditions That Are Too Clever

Clear beats clever:

```c
if (cars >= 0 && cars <= capacity) {
    printf("valid\n");
}
```

Do not compress logic until it becomes hard to read.

## Practice

Write a function:

```c
void print_temperature_label(int temperature);
```

Rules:

- below `0`: `freezing`
- `0` through `20`: `cold`
- `21` through `29`: `warm`
- `30` or above: `hot`

Call it with at least four different values.

---

[**Next ->** Loops and Repetition](./04-loops-and-repetition.md)  
[**<- Previous** Functions, Parameters, and Return Values](./02-functions-parameters-and-return-values.md)
