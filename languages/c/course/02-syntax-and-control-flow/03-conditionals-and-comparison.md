<h1 align="center">
    <img width="99" alt="C logo" src="../../../../_assets/logos/c.svg">
    <br>
    <b>C</b>
</h1>

[Home](../../../../README.md) / [C](../../README.md) / [Chapter 02](./README.md)

---

# Conditionals and Comparison

> Programs make decisions by checking whether a condition is true.

**You will learn:**
- How `if` and `else` work in C
- How comparison operators read
- How to think about program branches

**Before this page, you should know:** [Functions, Parameters, and Return Values](./02-functions-parameters-and-return-values.md)

---

## Basic `if`

```c
if (age >= 18) {
    printf("adult\n");
}
```

If the condition is true, the block runs.

## Common comparisons

- `==` equal to
- `!=` not equal to
- `>` greater than
- `<` less than
- `>=` greater than or equal to
- `<=` less than or equal to

## `if` and `else`

```c
if (score >= 50) {
    printf("pass\n");
} else {
    printf("fail\n");
}
```

> [!WARNING]
> In C, `=` assigns a value and `==` compares values. Mixing them up causes a lot of beginner bugs.

---

## Recap

- `if` chooses a path.
- Comparisons produce true-or-false style results.
- `else` handles the other path.

## Try it yourself

Write a small check that prints `warm` if a temperature is above `20` and `cold` otherwise.

---

[**Next ->** Loops and Repetition](./04-loops-and-repetition.md)
[**<- Previous** Functions, Parameters, and Return Values](./02-functions-parameters-and-return-values.md)


