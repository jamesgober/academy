<h1 align="center">
    <img width="99" alt="C logo" src="../../../../_assets/logos/c.svg">
    <br>
    <b>C</b>
</h1>

[Home](../../../../README.md) / [C](../../README.md) / [Chapter 02](./README.md)

---

# Loops and Repetition

> A loop repeats work so you do not have to write the same code again and again.

**You will learn:**
- How `for` and `while` loops work in C
- Why loops need a clear stopping rule
- How to avoid obvious infinite-loop mistakes

**Before this page, you should know:** [Conditionals and Comparison](./03-conditionals-and-comparison.md)

---

## `for` loop example

```c
for (int i = 0; i < 3; i++) {
    printf("%d\n", i);
}
```

## `while` loop example

```c
int count = 0;
while (count < 3) {
    printf("%d\n", count);
    count++;
}
```

## What matters most

A loop needs:
- a starting point
- a condition
- progress toward stopping

> [!IMPORTANT]
> If the condition never becomes false, the loop never stops.

---

## Recap

- Loops repeat work.
- `for` is useful for counting patterns.
- `while` is useful when repetition depends on a condition.

## Try it yourself

Write a loop that prints the numbers `1` through `5`.

---

[**Next ->** Chapter 02 Checkpoint](./05-chapter-02-checkpoint.md)
[**<- Previous** Conditionals and Comparison](./03-conditionals-and-comparison.md)


