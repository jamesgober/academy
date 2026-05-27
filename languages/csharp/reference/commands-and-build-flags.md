# Commands and Build Flags

Use these commands daily when working in C# projects.

## Core commands

```bash
dotnet new console -n app
dotnet build
dotnet run
dotnet test
dotnet clean
```

## Useful command variants

```bash
dotnet build -c Release
dotnet run --project src/App/App.csproj
dotnet test --filter Category=Unit
dotnet publish -c Release -o ./publish
```

## Diagnostic-focused build

```bash
dotnet build -v minimal
```

Use higher verbosity when troubleshooting build pipelines.

---

[← C# Reference](./README.md)
