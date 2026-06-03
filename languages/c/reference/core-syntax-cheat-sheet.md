# C Core Syntax Cheat Sheet

[Reference Index](./README.md) / [C](../README.md)

Use this page as a quick lookup for the beginner C syntax covered in
[Variables, Values, and Basic Types](../course/02-syntax-and-control-flow/01-variables-values-and-basic-types.md),
[Functions](../course/02-syntax-and-control-flow/02-functions-parameters-and-return-values.md),
[Conditionals](../course/02-syntax-and-control-flow/03-conditionals-and-comparison.md), and
[Loops](../course/02-syntax-and-control-flow/04-loops-and-repetition.md).

## Variables

```c
int count = 0;
char grade = 'A';
double average = 84.4;
```

Read a declaration as:

```text
type name = starting_value;
```

Beginner rule: initialize variables before reading them.

## Common Types

| Type | Use | Example |
|---|---|---|
| `int` | whole numbers | `int cars = 3;` |
| `char` | one character | `char grade = 'A';` |
| `float` | smaller decimal value | `float temp = 72.5f;` |
| `double` | common decimal value | `double average = 84.4;` |
| `size_t` | sizes and indexes | `size_t count = 4;` |

Use `double` for ordinary decimal examples. Use `size_t` for sizes returned by
functions such as `strlen`.

## `printf` Specifiers

```c
printf("Count: %d\n", count);
printf("Grade: %c\n", grade);
printf("Average: %.2f\n", average);
printf("Name: %s\n", name);
```

Common specifiers:

```text
%d     int
%c     char
%f     double in printf
%.2f   double with two digits after decimal
%s     C string
%zu    size_t
```

Format mismatches are risky in C. The compiler may warn, but `printf` still
trusts the format string.

## Functions

```c
int add(int left, int right) {
    return left + right;
}
```

Parts:

```text
int          return type
add          function name
int left     first parameter
int right    second parameter
return       sends result back
```

Use `void` when no value is returned:

```c
void print_header(void) {
    printf("Report\n");
}
```

## Function Prototype

Use a prototype when a function is called before its full definition:

```c
int add(int left, int right);

int main(void) {
    printf("%d\n", add(2, 3));
    return 0;
}

int add(int left, int right) {
    return left + right;
}
```

## Conditionals

```c
if (score >= 50) {
    printf("pass\n");
} else {
    printf("fail\n");
}
```

Common comparisons:

```text
==  equal
!=  not equal
>   greater than
<   less than
>=  greater than or equal
<=  less than or equal
```

Remember:

```text
=   assigns
==  compares
```

## Logical Operators

```c
if (age >= 16 && has_permit) {
    printf("can practice driving\n");
}

if (role == 1 || role == 2) {
    printf("staff access\n");
}

if (!is_open) {
    printf("closed\n");
}
```

```text
&&  and
||  or
!   not
```

## Loops

Counting loop:

```c
for (int i = 0; i < 10; i++) {
    printf("%d\n", i);
}
```

Condition-driven loop:

```c
int count = 0;

while (count < 10) {
    count++;
}
```

Every loop needs:

```text
starting state
condition
progress toward stopping
```

## Arrays Preview

```c
int scores[] = {88, 72, 95};

for (size_t i = 0; i < 3; i++) {
    printf("%d\n", scores[i]);
}
```

C arrays start at index `0`.

## Risk Notices

- Uninitialized variables can contain unpredictable values.
- `printf` format mismatches can produce undefined behavior.
- Integer division discards decimal parts unless you cast.
- `=` inside a condition is usually a bug when you meant `==`.
- Loops that do not make progress can run forever.
- Arrays do not automatically know their length after being passed to a
  function.

---

[Reference Index](./README.md) / [C](../README.md)
