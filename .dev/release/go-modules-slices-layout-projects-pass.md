# Go Modules, Slices, Layout, And Projects Pass

Date: 2026-05-29

## Summary

Expanded the Go track in the areas that were still too skeletal: module setup,
dependency files, slices/maps, project layout, final projects, and supporting
references.

## Course Areas Improved

- Rebuilt module/dependency guidance around `go.mod`, `go.sum`, import roots,
  `go get`, `go mod tidy`, direct vs indirect dependencies, and beginner
  dependency rules.
- Rebuilt arrays/slices/maps guidance around `append`, slicing, shared backing
  storage, `len`/`cap`, element removal, map comma-ok lookup, nil maps, and map
  order risks.
- Rebuilt the Chapter 02 checkpoint into a guided garage check-in program that
  integrates functions, slices, maps, loops, status lookup, accumulation, and
  formatted output.
- Rebuilt project layout guidance around `main`, reusable packages, `internal`,
  package naming, export boundaries, and architecture smells.
- Rebuilt the package/imports, interfaces, and Chapter 03 checkpoint lessons
  with module-root import paths, folder structure, exported/unexported names,
  implicit interface satisfaction, caller-owned interfaces, and a guided vehicle
  tracker package build.
- Rebuilt final project tutorials into guided garage CLI and worker-pool builds
  with folder structures, commands, tests, expected output, and sign-off
  questions.
- Expanded Go references for commands, core language containers, and
  packages/modules/layout with cross-links and risk notices.
- Expanded Go function and conditional references with syntax breakdowns,
  `(value, error)` patterns, variadic parameters, guard clauses, map lookup
  conditionals, switch behavior, and risk notices.
- Expanded Go structs/interfaces and concurrency references with receiver
  choices, constructors, export rules, implicit interface satisfaction,
  goroutines, WaitGroups, channels, closing, select, race risks, and shutdown
  prompts.
- Expanded Go error/lint, environment, and testing references with compiler and
  `go vet` message triage, `go env` explanations, cache notes, table-driven
  tests, focused test commands, race checks, and risk notices.
- Rebuilt Chapter 04 testing/tooling basics around real package files,
  `_test.go` structure, `testing.T`, `got`/`want`, table-driven tests,
  `t.Run`, failure output, `go test -run`, `go vet`, `gofmt`, `go doc`,
  package comments, and a tested status-package capstone with `QUALITY.md`.
- Rebuilt the beginner Go quality workflow into a repeatable `gofmt`, `go vet`,
  `go test`, focused-test, race-check, and failure-triage guide.
- Rebuilt the goroutines introduction around clear concurrency mental models,
  `sync.WaitGroup`, waiting versus starting, output ordering, loop arguments,
  and common beginner mistakes.

## Validation

- Go markdown broken-link scan: clean
- Go odd-code-fence scan: clean
- Go Mermaid block scan: clean
- Go stale nav/chrome/mojibake scan: clean

## Local Compile Note

The local machine does not currently have the `go` command on PATH, so Go code
snippets were not smoke-tested locally in this pass.
