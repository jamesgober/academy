<h1 align="center">
    <img width="99" alt="Go logo" src="../../../../_assets/logos/go.svg">
    <br>
    <b>Go</b>
</h1>

[Home](../../../../README.md) / [Go](../../README.md) / [Chapter 02](./README.md)

---

# Arrays, Slices, And Maps In Plain Language

> Go gives you a few container types. Arrays are fixed. Slices are the everyday
> flexible list. Maps are lookup tables. Most beginner Go code uses slices and
> maps constantly.

**You will learn:**
- What arrays are
- Why slices are more common
- How `append` works
- How slicing works
- How `len` and `cap` differ
- How maps store key-value pairs
- How comma-ok map lookup works
- Common slice and map mistakes

**Before this page, you should know:** [Loops And Repetition In Go](./04-loops-and-repetition-in-go.md)

---

## Arrays

An array has a fixed length.

```go
var scores [3]int

scores[0] = 10
scores[1] = 20
scores[2] = 30
```

This means:

```text
scores holds exactly 3 ints.
```

The length is part of the type.

```go
var a [3]int
var b [4]int
```

`a` and `b` have different types.

Beginner truth:

```text
Arrays exist, but slices are what you will use most.
```

---

## Slices

A slice is a flexible view over a sequence.

```go
names := []string{"Ana", "Bo", "Cam"}
```

Length:

```go
fmt.Println(len(names)) // 3
```

Index:

```go
fmt.Println(names[0]) // Ana
```

Loop:

```go
for index, name := range names {
    fmt.Println(index, name)
}
```

---

## Append

`append` returns a new slice value.

```go
names := []string{"Ana", "Bo"}
names = append(names, "Cam")
```

Important:

```text
Always use the returned slice from append.
```

Wrong:

```go
append(names, "Cam") // result ignored
```

Correct:

```go
names = append(names, "Cam")
```

Append multiple values:

```go
names = append(names, "Dee", "Eli")
```

Append another slice:

```go
more := []string{"Fay", "Gus"}
names = append(names, more...)
```

The `...` expands the slice into individual arguments.

---

## Slicing

```go
values := []int{10, 20, 30, 40, 50}
middle := values[1:4]
```

`middle` contains:

```text
20, 30, 40
```

Read:

```text
start at index 1
stop before index 4
```

Common forms:

```go
values[:3] // first three
values[2:] // from index 2 to end
values[:]  // whole slice
```

---

## Slices Share Backing Storage

This surprises beginners:

```go
values := []int{10, 20, 30, 40}
part := values[1:3]

part[0] = 99

fmt.Println(values) // [10 99 30 40]
```

Why?

```text
part is a view into the same underlying array.
```

If you need an independent copy:

```go
copyOfPart := append([]int(nil), part...)
```

---

## `len` And `cap`

```go
values := make([]int, 0, 5)

fmt.Println(len(values)) // 0
fmt.Println(cap(values)) // 5
```

Length:

```text
how many elements are currently in the slice
```

Capacity:

```text
how many elements can fit before Go likely needs a bigger backing array
```

Most beginner code cares about `len` more than `cap`.

Use capacity when you know roughly how many items you will append:

```go
names := make([]string, 0, 100)
```

---

## Remove From Slice

Remove item at index `i` when order matters:

```go
values = append(values[:i], values[i+1:]...)
```

Example:

```go
values := []int{10, 20, 30, 40}
i := 1
values = append(values[:i], values[i+1:]...)

fmt.Println(values) // [10 30 40]
```

Notice:

```text
This keeps order but shifts later elements.
```

---

## Maps

A map stores key-value pairs.

```go
ages := map[string]int{
    "Ana": 21,
    "Bo":  19,
}
```

Read:

```go
fmt.Println(ages["Ana"])
```

Set:

```go
ages["Cam"] = 25
```

Delete:

```go
delete(ages, "Bo")
```

---

## Comma-Ok Lookup

Map lookup returns the zero value when a key is missing.

```go
score := scores["missing"]
```

If `scores` is `map[string]int`, missing gives `0`.

But maybe the real score is `0`.

Use comma-ok:

```go
score, ok := scores["Ana"]
if !ok {
    fmt.Println("score not found")
    return
}

fmt.Println(score)
```

`ok` tells you whether the key existed.

---

## Nil Maps

A nil map can be read, but not written.

```go
var scores map[string]int

fmt.Println(scores["Ana"]) // ok, prints 0
scores["Ana"] = 10         // panic
```

Create a map before writing:

```go
scores := make(map[string]int)
scores["Ana"] = 10
```

---

## Real Example: Inventory

```go
package main

import "fmt"

type Product struct {
    SKU   string
    Name  string
    Stock int
}

func InStock(products []Product) []Product {
    result := make([]Product, 0, len(products))

    for _, product := range products {
        if product.Stock > 0 {
            result = append(result, product)
        }
    }

    return result
}

func IndexBySKU(products []Product) map[string]Product {
    bySKU := make(map[string]Product, len(products))

    for _, product := range products {
        bySKU[product.SKU] = product
    }

    return bySKU
}

func main() {
    products := []Product{
        {SKU: "KB-100", Name: "Keyboard", Stock: 3},
        {SKU: "MS-200", Name: "Mouse", Stock: 0},
    }

    available := InStock(products)
    fmt.Println(available)

    bySKU := IndexBySKU(products)

    if product, ok := bySKU["KB-100"]; ok {
        fmt.Println(product.Name)
    }
}
```

---

## Common Mistakes

### Mistake 1: Ignoring `append`

```go
append(values, 10)
```

Use:

```go
values = append(values, 10)
```

### Mistake 2: Forgetting Slices Can Share Storage

If changing one slice unexpectedly changes another, check whether one was sliced
from the other.

### Mistake 3: Writing To A Nil Map

Use `make(map[K]V)` or a map literal before assignment.

### Mistake 4: Assuming Map Order

Map iteration order is not guaranteed.

If you need sorted output, collect keys and sort them.

---

## Chapter Checkpoint

You should now be able to answer:

- Why are slices more common than arrays?
- What does `append` return?
- What does `values[1:4]` mean?
- Why can slices share storage?
- What is the difference between `len` and `cap`?
- How do you remove one slice element?
- What does comma-ok map lookup solve?
- Why can writing to a nil map panic?

---

## Recap

- Arrays have fixed length.
- Slices are the everyday flexible list.
- `append` returns the updated slice.
- Slicing uses start-inclusive, end-exclusive indexes.
- Slices can share backing storage.
- Maps store key-value pairs.
- Comma-ok lookup distinguishes missing keys from zero values.

## Try It Yourself

Create:

- a `Product` struct
- a slice of products
- a function that returns only in-stock products
- a map from SKU to product
- a lookup that prints a friendly missing message

---

[**Next ->** Chapter 02 Checkpoint](./06-chapter-02-checkpoint.md)  
[**<- Previous** Loops And Repetition In Go](./04-loops-and-repetition-in-go.md)
