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
- Rebuilt project layout guidance around `main`, reusable packages, `internal`,
  package naming, export boundaries, and architecture smells.
- Rebuilt final project tutorials into guided garage CLI and worker-pool builds
  with folder structures, commands, tests, expected output, and sign-off
  questions.
- Expanded Go references for commands, core language containers, and
  packages/modules/layout with cross-links and risk notices.

## Validation

- Go markdown broken-link scan: clean
- Go odd-code-fence scan: clean
- Go Mermaid block scan: clean
- Go stale nav/chrome/mojibake scan: clean

## Local Compile Note

The local machine does not currently have the `go` command on PATH, so Go code
snippets were not smoke-tested locally in this pass.
