<h1 align="center">
    <img width="99" alt="Go logo" src="../../../../_assets/logos/go.svg">
    <br>
    <b>Go</b>
</h1>

[Home](../../../../README.md) / [Go](../../README.md) / [Chapter 04](./README.md)

---

# Reading Test Failures And Debugging Faster

> A failed test is not an insult. It is a map. It tells you which behavior broke,
> where the test noticed, and what mismatch to investigate first.

**You will learn:**
- How to read `go test` failure output
- How subtest names help
- How to separate symptom from cause
- How to use `go test -run`
- How to debug with smaller cases
- How `go vet` output differs from tests

**Before this page, you should know:** [Table-Driven Tests Without Confusion](./02-table-driven-tests-without-confusion.md)

---

## Basic Failure Output

Example:

```text
--- FAIL: TestAdd (0.00s)
    add_test.go:14: Add(2, 3) = 6, want 5
FAIL
FAIL    example.com/calc-demo/calc    0.123s
```

Read:

| Part | Meaning |
|---|---|
| `TestAdd` | failing test |
| `add_test.go:14` | file and line |
| `Add(2, 3) = 6, want 5` | mismatch |
| package line | package that failed |

Start with the first failing message you understand.

---

## Subtest Failure Output

For table-driven tests:

```text
--- FAIL: TestCost (0.00s)
    --- FAIL: TestCost/large_package (0.00s)
        shipping_test.go:24: Cost(51) = 10, want 25
```

Read:

```text
The overall TestCost failed.
The large_package case failed.
The input was 51.
The function returned 10.
The expected value was 25.
```

This points you directly at the boundary between medium and large packages.

---

## Run One Test

Run only matching tests:

```bash
go test ./... -run TestCost
```

Run one subtest pattern:

```bash
go test ./... -run 'TestCost/large_package'
```

Use this when one failure is noisy and you want a tight loop.

Then run all tests before calling the fix done:

```bash
go test ./...
```

---

## Symptom Versus Cause

Symptom:

```text
Cost(51) = 10, want 25
```

Possible causes:

- condition uses `< 50` instead of `<= 50`
- cases are ordered wrong
- test expectation is wrong
- units are misunderstood

Do not edit randomly.

Open the function and compare the rule to the failing case.

---

## Debugging Order

1. Reproduce with `go test ./...`.
2. Run only the failing test with `-run`.
3. Read the failing input.
4. Inspect the smallest function involved.
5. Add a smaller test if the boundary is unclear.
6. Fix the cause.
7. Rerun one test.
8. Rerun all tests.

---

## Temporary Prints

You can use temporary prints while learning:

```go
fmt.Printf("weight=%d result=%d\n", tt.weight, got)
```

But do not leave noisy prints in tests.

Better long-term:

```go
t.Fatalf("Cost(%d) = %d, want %d", tt.weight, got, tt.want)
```

The failure message should already contain the useful debugging information.

---

## `go vet` Is Different

`go test` runs tests.

`go vet` checks suspicious code patterns.

Example:

```text
./main.go:27: fmt.Printf format %d has arg name of wrong type string
```

Read:

```text
main.go line 27
Printf format string expects an integer
but name is a string
```

Fix:

```go
fmt.Printf("%s\n", name)
```

Then rerun:

```bash
go vet ./...
go test ./...
```

---

## Common Mistakes

### Mistake 1: Bad Failure Messages

Weak:

```go
t.Fatal("failed")
```

Useful:

```go
t.Fatalf("Normalize(%q) = %q, want %q", input, got, want)
```

### Mistake 2: Only Running One Test Forever

Use one test while debugging, but run all tests before finishing.

### Mistake 3: Assuming The Code Is Wrong

Sometimes the test expectation is wrong.

Check the rule, not your ego.

---

## Chapter Checkpoint

You should now be able to answer:

- What does `file.go:14` tell you?
- Why do subtest names matter?
- What does `go test -run` do?
- Why should you rerun all tests after a fix?
- How is `go vet` different from `go test`?
- What belongs in a useful failure message?

---

## Recap

- Failed tests narrow the search.
- Failure messages should show input, got, and want.
- Subtests identify which case failed.
- `go test -run` creates a faster debugging loop.
- `go vet` finds suspicious code, not behavior failures.

## Try It Yourself

Make one table-driven test fail on purpose. Then write down:

- failing test name
- failing case name
- file and line
- got value
- want value
- likely cause

---

[**Next ->** Formatting, Vetting, And Documentation Tools](./04-formatting-vetting-and-documentation-tools.md)  
[**<- Previous** Table-Driven Tests Without Confusion](./02-table-driven-tests-without-confusion.md)
