# Commands and Build Flags

Use these commands daily when working in C# projects.

## Core commands

```bash
dotnet new console -n app
dotnet restore
dotnet build
dotnet run
dotnet test
dotnet clean
dotnet format
```

## Useful command variants

```bash
dotnet build -c Release
dotnet run --project src/App/App.csproj
dotnet test --filter Category=Unit
dotnet publish -c Release -o ./publish
dotnet build -warnaserror
dotnet test --logger "console;verbosity=detailed"
```

## Diagnostic-focused build

```bash
dotnet build -v minimal
dotnet build -v detailed
```

Use higher verbosity when troubleshooting build pipelines.

## Recommended local quality loop

```bash
dotnet restore
dotnet build -warnaserror
dotnet test
dotnet format --verify-no-changes
```

## Notes

- Use `-warnaserror` to keep warnings from silently accumulating.
- Use detailed test logger output when diagnosing flaky or environment-specific failures.

---

[C# Reference](./README.md)

