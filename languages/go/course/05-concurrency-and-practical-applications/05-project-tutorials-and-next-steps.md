<h1 align="center">
    <img width="99" alt="Go logo" src="../../../../_assets/logos/go.svg">
    <br>
    <b>Go</b>
</h1>

[Home](../../../../README.md) / [Go](../../README.md) / [Chapter 05](./README.md)

---

# Project Tutorials And Next Steps

> The end of the Go track should not be a vague checklist. You should leave with
> small projects you can build, test, explain, and extend.

**You will build:**
- a package-based garage status CLI
- a worker-pool job processor
- tests for core behavior
- a quality workflow using Go's built-in tools

**Before this page, you should know:** [A Small Worker Pattern](./04-a-small-worker-pattern.md)

---

## Project 1: Garage Status CLI

Goal:

```text
Read vehicle speeds, calculate status, print clear output.
```

Project:

```text
garage-app/
  go.mod
  main.go
  garage/
    vehicle.go
    status.go
    status_test.go
```

Create:

```bash
mkdir garage-app
cd garage-app
go mod init example.com/garage-app
mkdir garage
```

---

## Garage Package

`garage/vehicle.go`:

```go
package garage

type Vehicle struct {
    Plate string
    Speed int
}
```

`garage/status.go`:

```go
package garage

func Status(v Vehicle) string {
    switch {
    case v.Speed < 0:
        return "invalid"
    case v.Speed > 120:
        return "warning"
    default:
        return "ok"
    }
}
```

---

## Tests

`garage/status_test.go`:

```go
package garage

import "testing"

func TestStatus(t *testing.T) {
    tests := []struct {
        name string
        in   Vehicle
        want string
    }{
        {name: "normal speed", in: Vehicle{Plate: "A", Speed: 90}, want: "ok"},
        {name: "too fast", in: Vehicle{Plate: "B", Speed: 130}, want: "warning"},
        {name: "negative speed", in: Vehicle{Plate: "C", Speed: -1}, want: "invalid"},
    }

    for _, tt := range tests {
        t.Run(tt.name, func(t *testing.T) {
            got := Status(tt.in)

            if got != tt.want {
                t.Fatalf("Status(%+v) = %q, want %q", tt.in, got, tt.want)
            }
        })
    }
}
```

Run:

```bash
go test ./...
```

---

## Main Program

`main.go`:

```go
package main

import (
    "fmt"

    "example.com/garage-app/garage"
)

func main() {
    vehicles := []garage.Vehicle{
        {Plate: "ABC-123", Speed: 90},
        {Plate: "FAST-1", Speed: 140},
    }

    for _, vehicle := range vehicles {
        fmt.Printf("%s: %s\n", vehicle.Plate, garage.Status(vehicle))
    }
}
```

Run:

```bash
go run .
```

Expected:

```text
ABC-123: ok
FAST-1: warning
```

---

## Project 1 Quality Gate

Run:

```bash
gofmt -w .
go test ./...
go run .
go mod tidy
```

Sign-off questions:

- Why is `Status` in package `garage`?
- Why does `main` import `garage` using the module path?
- What does the table-driven test cover?
- What would you change if vehicle data came from a file?

---

## Project 2: Worker Pool

Goal:

```text
Process jobs concurrently and collect exactly one result per job.
```

Project:

```text
queue-demo/
  go.mod
  main.go
  worker/
    run.go
    run_test.go
```

Create:

```bash
mkdir queue-demo
cd queue-demo
go mod init example.com/queue-demo
mkdir worker
```

---

## Worker Package

`worker/run.go`:

```go
package worker

type Job struct {
    ID    int
    Value int
}

type Result struct {
    JobID int
    Value int
}

func Start(id int, jobs <-chan Job, results chan<- Result) {
    for job := range jobs {
        results <- Result{
            JobID: job.ID,
            Value: job.Value * 2,
        }
    }
}
```

Notice:

```text
jobs is receive-only.
results is send-only.
The worker exits when jobs is closed.
```

---

## Main Program

`main.go`:

```go
package main

import (
    "fmt"
    "sync"

    "example.com/queue-demo/worker"
)

func main() {
    jobs := make(chan worker.Job)
    results := make(chan worker.Result)

    var wg sync.WaitGroup

    for id := 1; id <= 3; id++ {
        wg.Add(1)

        go func(workerID int) {
            defer wg.Done()
            worker.Start(workerID, jobs, results)
        }(id)
    }

    go func() {
        for id := 1; id <= 5; id++ {
            jobs <- worker.Job{ID: id, Value: id * 10}
        }

        close(jobs)
        wg.Wait()
        close(results)
    }()

    for result := range results {
        fmt.Printf("job %d -> %d\n", result.JobID, result.Value)
    }
}
```

Flow model:

```text
main sends jobs
workers receive jobs
workers send results
main ranges over results
jobs closes first
workers finish
results closes last
```

---

## Worker Test

`worker/run_test.go`:

```go
package worker

import "testing"

func TestStartProcessesJobs(t *testing.T) {
    jobs := make(chan Job)
    results := make(chan Result)

    go func() {
        Start(1, jobs, results)
        close(results)
    }()

    jobs <- Job{ID: 10, Value: 7}
    close(jobs)

    result, ok := <-results
    if !ok {
        t.Fatal("expected one result")
    }

    if result.JobID != 10 || result.Value != 14 {
        t.Fatalf("result = %+v, want JobID 10 Value 14", result)
    }

    _, ok = <-results
    if ok {
        t.Fatal("expected results channel to close")
    }
}
```

Run:

```bash
go test ./...
```

---

## Project 2 Quality Gate

Run:

```bash
gofmt -w .
go test ./...
go run .
go mod tidy
```

Sign-off questions:

- Which goroutine closes `jobs`?
- Which goroutine closes `results`?
- Why should workers not close `results` themselves?
- How does the program avoid waiting forever?
- How many results should five jobs produce?

---

## Next Steps

After these projects, practice:

- reading CSV with `encoding/csv`
- serving HTTP with `net/http`
- JSON encode/decode with `encoding/json`
- context cancellation with `context`
- more table-driven tests
- benchmarking with `go test -bench`

Do not rush to frameworks. Go's standard library can carry you a long way.

---

## Final Go Sign-Off

You are ready to leave the beginner Go track when you can:

- create a module
- explain `go.mod` and `go.sum`
- split code into packages
- export only necessary names
- write table-driven tests
- use slices and maps confidently
- explain goroutines and channels in plain language
- close channels from the sending side
- run `gofmt`, `go test ./...`, `go run .`, and `go mod tidy`
- explain the project structure without hand-waving

---

[**Next ->** Go Reference](../../reference/README.md)  
[**<- Previous** A Small Worker Pattern](./04-a-small-worker-pattern.md)
