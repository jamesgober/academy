<h1 align="center">
    <img width="99" alt="C# logo" src="../../../../_assets/logos/cs.svg">
    <br>
    <b>C#</b>
</h1>

[Home](../../../../README.md) / [C#](../../README.md) / [Chapter 01](./README.md)

---

# Your First C# Program

> Build and run a minimal C# app so you can trust your environment.

**You will learn:**
- How a minimal C# console app is structured
- What `Main` does
- How to run the program with `dotnet run`

**Before this page, you should know:** .NET SDK is installed and recognized by terminal.

---

## Hello C#

```csharp
using System;

class Program
{
    static void Main()
    {
        Console.WriteLine("Hello, C#");
    }
}
```

Key parts:
- `using System;` imports common framework types.
- `Main` is the program entry point.
- `Console.WriteLine` prints text to terminal.

## Run it

```bash
dotnet new console -n hello-csharp
cd hello-csharp
dotnet run
```

Expected output:

```text
Hello, C#
```

---

## Recap

- C# console apps start in `Main`.
- `Console.WriteLine` is the basic output API.
- `dotnet run` builds and executes your project.

## Try it yourself

Change output to include your name and rerun the project.

---

[**Next ->** Project Commands: new, build, run](./03-project-commands-new-build-run.md)  
[**<- Previous** Installing the .NET SDK](./01-installing-the-dotnet-sdk.md)


