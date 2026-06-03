<h1 align="center">
    <img width="99" alt="C logo" src="../../../../_assets/logos/c.svg">
    <br>
    <b>C</b>
</h1>

[Home](../../../../README.md) / [C](../../README.md) / [Chapter 02](./README.md)

---

# Variables, Values, And Basic Types

Variables let C programs store values and use them again later.

In C, every variable has a type. The type tells the compiler how much memory the
value needs and how operations should treat that value.

## A Variable In Plain Language

```c
int age = 30;
```

Read it as:

```text
create a variable named age
store the integer value 30 in it
```

Parts:

```text
int   type
age   name
=     assignment
30    value
;     end of statement
```

## Common Beginner Types

```c
int count = 5;
char grade = 'A';
float temperature = 72.5f;
double average = 84.4;
```

Beginner mental model:

```text
int     whole number
char    one character
float   smaller decimal number
double  common decimal number with more precision
```

Prefer `double` for ordinary decimal calculations unless a lesson specifically
uses `float`.

## Printing Values

`printf` uses format specifiers.

```c
#include <stdio.h>

int main(void) {
    int count = 5;
    char grade = 'A';
    double average = 84.4;

    printf("Count: %d\n", count);
    printf("Grade: %c\n", grade);
    printf("Average: %.1f\n", average);

    return 0;
}
```

Common specifiers:

```text
%d    int
%c    char
%f    double in printf
%.2f  double with two digits after the decimal
%s    C string
```

The format string and the values must match. If they do not, C may print
garbage or behave unpredictably.

## Declaration Versus Assignment

Declaration creates the variable:

```c
int cars;
```

Assignment stores a value:

```c
cars = 3;
```

You can do both at once:

```c
int cars = 3;
```

Beginner rule: initialize variables when you create them.

Bad:

```c
int total;
printf("%d\n", total);
```

`total` has an indeterminate value.

Good:

```c
int total = 0;
printf("%d\n", total);
```

## Updating Values

```c
int cars = 0;

cars = cars + 1;
cars += 1;
cars++;
```

All three updates add one, but they read slightly differently.

Use the clearest form for the situation.

## Integer Division

This surprises many beginners:

```c
int total = 5;
int count = 2;
double average = total / count;
```

`average` becomes `2.0`, not `2.5`, because `total / count` performs integer
division before storing the result.

Fix it by converting one side:

```c
double average = (double)total / count;
```

## Constants

Use `const` when a variable should not change:

```c
const int capacity = 10;
```

The compiler will reject later attempts to assign a new value:

```c
capacity = 12; // error
```

Constants make intent clearer.

## Small Program: Garage Capacity

```c
#include <stdio.h>

int main(void) {
    const int capacity = 5;
    int cars = 3;
    int spaces = capacity - cars;
    double used_percent = ((double)cars / capacity) * 100.0;

    printf("Capacity: %d\n", capacity);
    printf("Cars: %d\n", cars);
    printf("Spaces: %d\n", spaces);
    printf("Used: %.1f%%\n", used_percent);

    return 0;
}
```

`%%` prints a literal percent sign.

## Common Beginner Mistakes

### Forgetting To Initialize

Always give a variable a known starting value before reading it.

### Using The Wrong `printf` Specifier

Match the value:

```c
printf("%d\n", cars);      // int
printf("%.2f\n", price);   // double
printf("%c\n", grade);     // char
```

### Expecting Decimal Results From Integer Division

If both operands are integers, C uses integer division.

Convert intentionally:

```c
double ratio = (double)used / capacity;
```

## Practice

Write a program that stores:

- a garage capacity
- a current car count
- a price per hour
- a letter grade for the garage

Print all values, calculate remaining spaces, and calculate the percent full.

---

[**Next ->** Functions, Parameters, and Return Values](./02-functions-parameters-and-return-values.md)  
[**<- Previous** Chapter 01](../01-getting-started/README.md)
