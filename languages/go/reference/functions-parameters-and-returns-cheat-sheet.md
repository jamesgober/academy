<h1 align="center">
    <img width="99" alt="Go logo" src="../../../_assets/logos/go.svg">
    <br>
    <b>Go</b>
</h1>

[Home](../../../README.md) / [Go](../README.md) / [Reference](./README.md)

---

# Functions, Parameters, And Returns Cheat Sheet

Use this page when you need to remember Go function syntax or choose a clean
function shape. For the lesson version, see [Functions, Parameters, and Returns](../course/02-core-language-basics/02-functions-parameters-and-returns.md).

## Basic Function

```go
func greet(name string) string {
    return "hi " + name
}
```

Read it as:

```text
func       define a function
greet      function name
name       parameter name
string     parameter type
string     return type
return     send a value back
```

Call it:

```go
message := greet("Ada")
fmt.Println(message)
```

## Multiple Parameters

When consecutive parameters share a type, Go lets you write the type once:

```go
func add(left, right int) int {
    return left + right
}
```

This is the same idea as:

```go
func add(left int, right int) int {
    return left + right
}
```

Use whichever is clearer for the function.

## No Return Value

```go
func printHeader(title string) {
    fmt.Println(title)
    fmt.Println("------------")
}
```

If there is no return type after the parameter list, the function returns
nothing.

## Multiple Return Values

Go commonly returns more than one value:

```go
func divide(a, b int) (int, int) {
    return a / b, a % b
}
```

Call it:

```go
quotient, remainder := divide(10, 3)
```

Multiple returns are also used for operation results plus errors:

```go
func parsePort(raw string) (int, error) {
    port, err := strconv.Atoi(raw)
    if err != nil {
        return 0, err
    }

    return port, nil
}
```

## The `(value, error)` Pattern

Common production shape:

```go
func loadConfig(path string) (Config, error) {
    // ...
}
```

Read it as:

```text
return the useful value if things work
return an error if something fails
```

Caller pattern:

```go
config, err := loadConfig("config.json")
if err != nil {
    return err
}

fmt.Println(config.Name)
```

Do not ignore `err` unless you have a very specific reason.

## Variadic Parameters

A variadic parameter accepts zero or more values:

```go
func sum(values ...int) int {
    total := 0

    for _, value := range values {
        total += value
    }

    return total
}
```

Call it:

```go
fmt.Println(sum(1, 2, 3))
```

Inside the function, `values` behaves like a slice.

## Named Return Values

Go supports named returns:

```go
func splitName(full string) (first string, last string) {
    parts := strings.Fields(full)
    if len(parts) >= 1 {
        first = parts[0]
    }
    if len(parts) >= 2 {
        last = parts[1]
    }
    return
}
```

Use named returns sparingly. They can clarify short functions, but long
functions with naked `return` statements can become hard to read.

## Design Prompts

Ask these before writing a function:

- Does this function do one clear job?
- Is the function name a verb or clear action?
- Are parameter names specific?
- Is the return value obvious from the name?
- Should failure be returned as `error` instead of causing a panic?
- Would a small struct make many parameters clearer?

## Risk Notices

- Avoid functions with many unrelated parameters.
- Avoid ignoring returned errors.
- Avoid using `panic` for normal user or file errors.
- Avoid named returns in long functions unless they genuinely clarify the code.
- Avoid clever one-line functions when a readable body would teach more.

## Related Lessons

- [Functions, Parameters, and Returns](../course/02-core-language-basics/02-functions-parameters-and-returns.md)
- [Chapter 02 Checkpoint](../course/02-core-language-basics/06-chapter-02-checkpoint.md)
- [Errors, Warnings, and Linting Guide](./errors-warnings-and-linting-guide.md)

---

[Reference Index](./README.md) / [Go](../README.md) / [Home](../../../README.md)
