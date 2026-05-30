<h1 align="center">
    <img width="99" alt="Go logo" src="../../../../_assets/logos/go.svg">
    <br>
    <b>Go</b>
</h1>

[Home](../../../../README.md) / [Go](../../README.md) / [Chapter 04](./README.md)

---

# A Beginner-Friendly Go Quality Workflow

> A quality workflow is just a repeatable order for staying out of trouble.

**You will learn:**
- A simple pre-commit command order
- Why quality checks belong during development, not only at the end
- How this chapter connects to CI later

**Before this page, you should know:** [Formatting, Vetting, and Documentation Tools](./04-formatting-vetting-and-documentation-tools.md)

---

## Suggested order

1. `go fmt ./...`
2. `go vet ./...`
3. `go test ./...`

## Why this order

- formatting removes style noise first
- vet flags suspicious code early
- tests check behavior after the code looks structurally sound

## Release-readiness mindset

Before you call code ready:
- formatting is clean
- vet is clean
- tests pass
- the change is understandable to another developer

---

## Recap

- Use one repeatable workflow.
- Quality checks should be normal, not dramatic.
- A good local workflow makes CI easier later.

## Try it yourself

Write the three-command workflow on a scratch note and run it every time you change a practice project.

---

[**Next ->** Chapter 04 Capstone](./06-chapter-04-capstone.md)
[**<- Previous** Formatting, Vetting, and Documentation Tools](./04-formatting-vetting-and-documentation-tools.md)


