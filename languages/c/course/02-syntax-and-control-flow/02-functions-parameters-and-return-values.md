<h1 align="center">
    <img width="99" alt="C logo" src="../../../../_assets/logos/c.svg">
    <br>
    <b>C</b>
</h1>

[Home](../../../../README.md) / [C](../../README.md) / [Chapter 02](./README.md)

---

# Functions, Parameters, and Return Values

> Functions package one job into a reusable named block of C code.

**You will learn:**
- What a function definition looks like
- What parameters are
- How return values work in C

**Before this page, you should know:** [Variables, Values, and Basic Types](./01-variables-values-and-basic-types.md)

---

## Simple function

```c
int add(int left, int right) {
    return left + right;
}
```

Read it slowly:
- `int` at the front means the function returns an integer
- `add` is the function name
- `left` and `right` are parameters
- `return` sends the result back

## Calling the function

```c
int total = add(2, 3);
```

## Mental model

A function takes input, does work, and may return output.
That same model applies across most languages.

---

## Recap

- Functions package reusable behavior.
- Parameters describe inputs.
- Return values send results back.

## Try it yourself

Write a function named `triple` that returns three times an integer input.

---

[**Next ->** Conditionals and Comparison](./03-conditionals-and-comparison.md)
[**<- Previous** Variables, Values, and Basic Types](./01-variables-values-and-basic-types.md)


