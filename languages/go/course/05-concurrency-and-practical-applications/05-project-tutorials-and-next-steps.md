<h1 align="center">
    <img width="99" alt="Go logo" src="../../../../_assets/logos/go.svg">
    <br>
    <b>Go</b>
</h1>

<div align="center">

[Home](../../../../README.md) · [Go](../../README.md) · [Chapter 05](./README.md)

</div>

---

# Project Tutorials and Next Steps

> Finish the Go track by applying the ideas in small, realistic projects.

## Project tutorial 1

Build a command-line garage status app that:
- reads a list of vehicles
- uses packages for organization
- includes tests for core behavior
- documents the local quality workflow

Suggested structure:

```text
garage-app/
├── go.mod
├── main.go
├── garage/
│   ├── status.go
│   └── status_test.go
└── cmd/
    └── parse.go
```

Core code sketch:

```go
package garage

func Status(speed int) string {
    if speed > 120 {
        return "warning"
    }
    return "ok"
}
```

Expected outcomes:
- CLI prints one status per input record
- `go test ./...` passes
- package split is easy to explain

## Project tutorial 2

Build a small job queue demo that:
- uses goroutines for workers
- uses channels for results
- keeps the concurrency flow diagrammed
- explains one concurrency risk and how the design avoids it

Suggested structure:

```text
queue-demo/
├── go.mod
├── main.go
└── worker/
    ├── run.go
    └── run_test.go
```

Worker sketch:

```go
func Worker(jobs <-chan int, results chan<- int) {
    for j := range jobs {
        results <- j * 2
    }
}
```

Expected outcomes:
- all submitted jobs produce exactly one result
- no goroutine leak after channel close
- flow diagram matches runtime behavior

## Reviewer checklist

- Is the project structure intentional?
- Are tests part of the workflow?
- Is concurrency used only where it helps?
- Can a new learner explain the flow back in plain language?
- Are expected outcomes verifiable from command output?

---

## Next

Use the Go reference for quick lookup, or continue to the next language track.

---

<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← A Small Worker Pattern](./04-a-small-worker-pattern.md) | [Chapter 05](./README.md) · [Go](../../README.md) · [Home](../../../../README.md) | [Go Reference →](../../reference/README.md) |

</div>
