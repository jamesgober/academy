<h1 align="center">
    <img width="99" alt="Go logo" src="../../../../_assets/logos/go.svg">
    <br>
    <b>Go</b>
</h1>

[Home](../../../../README.md) / [Go](../../README.md) / [Chapter 02](./README.md)

---

# Loops and Repetition in Go

> A loop repeats work so you do not have to write the same code over and over.

**You will learn:**
- How Go uses `for` for looping
- How counting loops differ from condition loops
- Why loops can go wrong when the stop condition is unclear

**Before this page, you should know:** [Conditionals and Comparison](./03-conditionals-and-comparison.md)

---

## Go uses `for`

Go does not have a separate `while` keyword.
It uses `for` for different loop styles.

```go
for i := 0; i < 3; i++ {
    fmt.Println(i)
}
```

`i` is just a common variable name for index/counter loops.
You can name it `index`, `step`, or anything meaningful to your code.

Expected output:

```text
0
1
2
```

## Condition-style loop

```go
count := 0
for count < 3 {
    fmt.Println(count)
    count++
}
```

## Why loops matter

Loops help you process lists, repeat checks, and build up results.

> [!WARNING]
> If the condition never becomes false, your loop will keep running forever.

---

## Recap

- Go uses `for` for repetition.
- A loop needs a clear stopping rule.
- Repetition is useful, but only when the control flow stays readable.

## Try it yourself

Write a loop that prints the numbers `1` through `5`.

---

[**Next ->** Arrays, Slices, and Maps in Plain Language](./05-arrays-slices-and-maps-in-plain-language.md)
[**<- Previous** Conditionals and Comparison](./03-conditionals-and-comparison.md)


