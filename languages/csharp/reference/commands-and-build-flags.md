# C# Commands And Build Flags

[C# Reference](./README.md) / [C#](../README.md)

Use this page when you need the daily .NET command workflow. For the guided
lesson, see [Project Commands: new, build, run](../course/01-getting-started/03-project-commands-new-build-run.md).

## Create Projects

```bash
dotnet new console -n app
dotnet new classlib -n App.Core
dotnet new xunit -n App.Tests
dotnet new sln -n App
```

List templates:

```bash
dotnet new list
```

## Restore

```bash
dotnet restore
```

Restores NuGet packages listed by the project.

Usually `build`, `run`, and `test` restore automatically when needed, but
explicit restore is useful in CI or troubleshooting.

## Build

```bash
dotnet build
dotnet build -c Release
dotnet build -warnaserror
```

Use `-warnaserror` when you want warnings to fail the build.

Diagnostic verbosity:

```bash
dotnet build -v minimal
dotnet build -v detailed
```

Use detailed verbosity only when diagnosing build problems; it can be noisy.

## Run

```bash
dotnet run
dotnet run --project src/App/App.csproj
dotnet run -- --name Ada
```

Arguments after `--` are passed to your app, not to `dotnet`.

Relative file paths in your app depend on the working directory. If your app
uses `inventory.json`, run from the folder where that file exists or build a
clear path in code.

## Test

```bash
dotnet test
dotnet test --filter FullyQualifiedName~OrderService
dotnet test --filter Category=Unit
dotnet test --logger "console;verbosity=detailed"
```

Use filters for focused work. Run the full suite before calling work done.

## Format

```bash
dotnet format
dotnet format --verify-no-changes
```

Use `--verify-no-changes` in quality gates or CI to fail when formatting would
change files.

## Publish

```bash
dotnet publish -c Release -o ./publish
```

Publish creates deployable output. It is not the same as ordinary local run.

## Clean

```bash
dotnet clean
```

Removes build outputs. Useful when generated files seem stale or confusing.

## Recommended Local Quality Loop

```bash
dotnet restore
dotnet build -warnaserror
dotnet test
dotnet format --verify-no-changes
```

For a small practice app, `dotnet build` then `dotnet run` is enough. For serious
work, use the full loop.

## Risk Notices

- Do not ignore warnings permanently.
- Do not rely only on `dotnet run`; it proves less than tests.
- Do not assume `--project` changes your app's working directory.
- Do not use detailed verbosity as normal output; save it for diagnosis.
- Do not publish Debug builds unless you specifically need them.

---

[C# Reference](./README.md) / [C#](../README.md)
