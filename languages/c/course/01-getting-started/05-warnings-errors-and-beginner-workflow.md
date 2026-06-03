<h1 align="center">
    <img width="99" alt="C logo" src="../../../../_assets/logos/c.svg">
    <br>
    <b>C</b>
</h1>

[Home](../../../../README.md) / [C](../../README.md) / [Chapter 01](./README.md)

---

# Warnings, Errors, And Beginner Workflow

In C, warnings matter. Ignoring them early teaches bad habits fast.

C gives you a lot of power and not much protection. Compiler warnings are one
of the earliest guardrails you get.

## Errors Versus Warnings

An error stops the build:

```text
error: expected ';' after expression
```

A warning means the compiler can still build, but it sees something suspicious:

```text
warning: variable 'count' is uninitialized when used here
```

Beginner rule:

```text
fix errors first
fix warnings next
run only after the build is clean
```

## Use Strict Warning Flags

Compile practice programs with:

```bash
cc -std=c17 -Wall -Wextra -Wpedantic -g main.c -o main
```

or:

```bash
clang -std=c17 -Wall -Wextra -Wpedantic -g main.c -o main
```

These flags ask the compiler to be more helpful:

```text
-Wall       many common warnings
-Wextra     additional warnings
-Wpedantic  non-standard C warnings
-g          debug information
```

## Read Diagnostics In Order

A diagnostic usually tells you:

```text
file
line
column
severity
message
```

Example:

```text
main.c:6:5: warning: variable 'total' is uninitialized when used here
```

Read it as:

```text
In main.c around line 6, total was used before receiving a known value.
```

## Fix The First Error First

One typo can create many confusing follow-up messages.

Use this loop:

```text
1. Compile.
2. Read the first error.
3. Fix the smallest likely cause.
4. Compile again.
5. Repeat.
6. Handle warnings.
7. Run the program.
```

Do not try to fix every message at once.

## Warning Example: Unused Variable

```c
#include <stdio.h>

int main(void) {
    int count = 3;
    int unused = 10;

    printf("%d\n", count);
    return 0;
}
```

The compiler may warn that `unused` is never used.

Fix it by removing the variable or using it for a real purpose.

## Warning Example: Uninitialized Value

Bad:

```c
#include <stdio.h>

int main(void) {
    int total;
    printf("%d\n", total);
    return 0;
}
```

`total` has no known starting value.

Good:

```c
int total = 0;
printf("%d\n", total);
```

## Warning Example: Format Mismatch

Bad:

```c
double price = 19.99;
printf("%d\n", price);
```

`%d` is for `int`, not `double`.

Good:

```c
double price = 19.99;
printf("%.2f\n", price);
```

Format mismatches are especially risky in C because `printf` trusts you.

## Beginner-Safe Compile Loop

Use this every time:

```text
edit one small thing
save
compile with warnings
fix first error
fix warnings
run
read output
repeat
```

This loop keeps problems small enough to understand.

## Practice

Create this file:

```c
#include <stdio.h>

int main(void) {
    int cars = 3;
    int unused = 9;

    printf("Cars: %d\n", cars);
    return 0;
}
```

Compile with:

```bash
cc -std=c17 -Wall -Wextra -Wpedantic -g main.c -o main
```

Find the warning. Fix it. Compile again.

Then remove a semicolon, compile, read the first error, and fix it.

## What You Should Be Able To Explain

Before Chapter 02, make sure you can explain:

- what an error is
- what a warning is
- why warnings matter in C
- why strict flags are useful
- why the first error matters most
- why a clean build comes before running

---

[**Next ->** Chapter 02](../02-syntax-and-control-flow/README.md)  
[**<- Previous** What Source Files and Executables Are](./04-what-source-files-and-executables-are.md)
