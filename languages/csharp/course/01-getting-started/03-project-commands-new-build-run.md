<h1 align="center">
    <img width="99" alt="C# logo" src="../../../../_assets/logos/cs.svg">
    <br>
    <b>C#</b>
</h1>

[Home](../../../../README.md) / [C#](../../README.md) / [Chapter 01](./README.md)

---

# Project Commands: new, build, run

> Learn the three commands you will use daily in C# projects.

**You will learn:**
- What `dotnet new`, `dotnet build`, and `dotnet run` do
- Why build and run are different steps
- A clean daily command workflow

**Before this page, you should know:** how to run a hello world console app.

---

## Core command loop

```bash
dotnet new console -n app-demo
dotnet build
dotnet run
```

What each does:
- `dotnet new` creates project files.
- `dotnet build` compiles and checks for errors.
- `dotnet run` builds if needed, then executes.

## Useful companions

```bash
dotnet clean
dotnet test
```

- `dotnet clean` removes build outputs.
- `dotnet test` runs test projects.

> [!TIP]
> Use `dotnet build` first when debugging compile errors. It gives cleaner diagnostics than repeated full runs.

---

## Recap

- `new` creates, `build` compiles, `run` executes.
- Use build first when troubleshooting compiler issues.
- Keep commands consistent across projects.

## Try it yourself

Create a second console project and run `new`, `build`, and `run` in order.

---

[**Next ->** Reading Errors and Warnings](./04-reading-errors-and-warnings.md)  
[**<- Previous** Your First C# Program](./02-your-first-csharp-program.md)


