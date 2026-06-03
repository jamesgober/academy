<h1 align="center">
    <img width="99" alt="C logo" src="../../../../_assets/logos/c.svg">
    <br>
    <b>C</b>
</h1>

[Home](../../../../README.md) / [C](../../README.md) / [Chapter 02](./README.md)

---

# Functions, Parameters, And Return Values

A function packages one job behind a name.

Without functions, every program turns into one long pile of statements. With
functions, you can name ideas:

```text
calculate_total
print_status
is_passing
```

Good function names make code easier to read because they say what the program
is trying to do.

## A First Function

```c
int add(int left, int right) {
    return left + right;
}
```

Read the function header from left to right:

```text
int      return type
add      function name
int left first parameter
int right second parameter
```

The body is inside braces:

```c
{
    return left + right;
}
```

`return` sends a value back to the caller.

## Calling A Function

```c
int total = add(2, 3);
```

Visual model:

```text
caller gives arguments
        |
        v
add(2, 3)
        |
        v
left = 2, right = 3
        |
        v
return 5
        |
        v
total = 5
```

Parameters are the names inside the function definition. Arguments are the
actual values passed during a call.

```c
int add(int left, int right)
```

`left` and `right` are parameters.

```c
add(2, 3)
```

`2` and `3` are arguments.

## Complete Example

```c
#include <stdio.h>

int add(int left, int right) {
    return left + right;
}

int main(void) {
    int apples = 4;
    int oranges = 3;
    int fruit_count = add(apples, oranges);

    printf("Fruit count: %d\n", fruit_count);

    return 0;
}
```

Output:

```text
Fruit count: 7
```

## Why The Function Must Be Known Before Use

C reads your file from top to bottom. If `main` calls a function before the
compiler has seen it, you need a function prototype.

This works because `add` is defined before `main`:

```c
int add(int left, int right) {
    return left + right;
}

int main(void) {
    int total = add(2, 3);
    return 0;
}
```

This uses a prototype:

```c
#include <stdio.h>

int add(int left, int right);

int main(void) {
    int total = add(2, 3);
    printf("%d\n", total);
    return 0;
}

int add(int left, int right) {
    return left + right;
}
```

The prototype tells the compiler:

```text
A function named add exists.
It takes two int values.
It returns an int.
The full body appears later.
```

## `void` Return Type

Some functions do work but do not return a value.

```c
void print_header(void) {
    printf("Garage report\n");
    printf("-------------\n");
}
```

Use `void` as the return type when there is no result to send back.

Use `void` in the parameter list when the function accepts no arguments.

```text
void print_header(void)
^    ^ no parameters
| no return value
```

## Realistic Example: Price With Tax

```c
#include <stdio.h>

double price_with_tax(double price, double tax_rate) {
    return price + (price * tax_rate);
}

int main(void) {
    double subtotal = 25.00;
    double tax_rate = 0.07;
    double total = price_with_tax(subtotal, tax_rate);

    printf("Subtotal: %.2f\n", subtotal);
    printf("Total: %.2f\n", total);

    return 0;
}
```

`%.2f` prints a floating-point number with two digits after the decimal point.

## Common Beginner Mistakes

### Forgetting The Return Value

This function promises to return `int`, but does not:

```c
int add(int left, int right) {
    left + right;
}
```

Fix it:

```c
int add(int left, int right) {
    return left + right;
}
```

### Mixing Up Printing And Returning

Printing shows something to the user.

Returning sends a value back to code.

```c
int add(int left, int right) {
    return left + right;
}
```

```c
printf("%d\n", add(2, 3));
```

Keep those ideas separate.

### Using A Function Before Declaring It

Modern C expects the compiler to know a function before it is called.

Fix this by:

- defining the function above `main`, or
- writing a prototype above `main`

## Practice

Write these functions:

```c
int triple(int value) {
    return value * 3;
}

int is_even(int value) {
    return value % 2 == 0;
}

void print_result(int value) {
    printf("Result: %d\n", value);
}
```

Then call them from `main`.

Compile with warnings enabled and fix every warning.

---

[**Next ->** Conditionals and Comparison](./03-conditionals-and-comparison.md)  
[**<- Previous** Variables, Values, and Basic Types](./01-variables-values-and-basic-types.md)
