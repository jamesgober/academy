<h1 align="center">
    <img width="99" alt="C logo" src="../../../../_assets/logos/c.svg">
    <br>
    <b>C</b>
</h1>

[Home](../../../../README.md) / [C](../../README.md) / [Chapter 02](./README.md)

---

# Loops And Repetition

A loop repeats work.

Without loops:

```c
printf("1\n");
printf("2\n");
printf("3\n");
```

With a loop:

```c
for (int number = 1; number <= 3; number++) {
    printf("%d\n", number);
}
```

Loops are how programs process lists, retry work, scan input, count things, and
repeat until a condition changes.

## The Three Parts Of A Counting Loop

```c
for (int i = 0; i < 3; i++) {
    printf("%d\n", i);
}
```

Read it like this:

```text
int i = 0   start at zero
i < 3       keep going while this is true
i++         add one after each loop body
```

Output:

```text
0
1
2
```

Notice it does not print `3`. The loop stops when `i < 3` becomes false.

## Visual Model

```text
i = 0 -> condition true  -> print 0 -> i becomes 1
i = 1 -> condition true  -> print 1 -> i becomes 2
i = 2 -> condition true  -> print 2 -> i becomes 3
i = 3 -> condition false -> stop
```

## Counting From 1 To 5

```c
for (int number = 1; number <= 5; number++) {
    printf("%d\n", number);
}
```

Use names that match the meaning. `number` is clearer than `i` when the variable
represents the number you are printing.

## `while` Loops

A `while` loop repeats while a condition is true.

```c
int count = 0;

while (count < 3) {
    printf("%d\n", count);
    count++;
}
```

Use `while` when the stopping point depends on a condition more than a simple
counting pattern.

## Avoid Infinite Loops

This loop never changes `count`:

```c
int count = 0;

while (count < 3) {
    printf("%d\n", count);
}
```

`count` stays `0`, so `count < 3` is always true.

Fix it by making progress:

```c
int count = 0;

while (count < 3) {
    printf("%d\n", count);
    count++;
}
```

Every loop needs:

- a starting state
- a condition
- progress toward stopping

## Summing Values

Loops often build a result over time.

```c
#include <stdio.h>

int main(void) {
    int total = 0;

    for (int number = 1; number <= 5; number++) {
        total += number;
    }

    printf("Total: %d\n", total);
    return 0;
}
```

Visual model:

```text
total starts at 0
add 1 -> 1
add 2 -> 3
add 3 -> 6
add 4 -> 10
add 5 -> 15
```

## Looping Over An Array Preview

Arrays get a full treatment later, but this simple preview shows why loops are
useful:

```c
#include <stdio.h>

int main(void) {
    int scores[] = {88, 72, 95, 100};
    int count = 4;
    int total = 0;

    for (int i = 0; i < count; i++) {
        total += scores[i];
    }

    printf("Total: %d\n", total);
    return 0;
}
```

The loop visits indexes `0`, `1`, `2`, and `3`.

```text
scores[0] -> 88
scores[1] -> 72
scores[2] -> 95
scores[3] -> 100
```

C arrays start at index `0`, not `1`.

## `break` And `continue`

`break` exits a loop early:

```c
for (int number = 1; number <= 10; number++) {
    if (number == 5) {
        break;
    }

    printf("%d\n", number);
}
```

`continue` skips to the next iteration:

```c
for (int number = 1; number <= 5; number++) {
    if (number == 3) {
        continue;
    }

    printf("%d\n", number);
}
```

Use them sparingly. They are helpful, but too many early exits can make a loop
hard to follow.

## Practice

Write a program that:

- prints numbers `1` through `5`
- calculates the total of those numbers
- prints whether the total is greater than `10`

Expected output:

```text
1
2
3
4
5
Total: 15
Total is greater than 10
```

Compile after each piece.

---

[**Next ->** Chapter 02 Checkpoint](./05-chapter-02-checkpoint.md)  
[**<- Previous** Conditionals and Comparison](./03-conditionals-and-comparison.md)
