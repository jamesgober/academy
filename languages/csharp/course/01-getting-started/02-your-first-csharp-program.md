<h1 align="center">
    <img width="99" alt="C# logo" src="../../../../_assets/logos/cs.svg">
    <br>
    <b>C#</b>
</h1>

[Home](../../../../README.md) / [C#](../../README.md) / [Chapter 01](./README.md)

---

# Your First C# Program

C# programs usually live inside .NET projects. That means your first program is
not just one source file. It is a small folder with a project file, source code,
and build output.

The goal of this lesson is to make that feel normal.

## Create A Console Project

Run:

```bash
dotnet new console -n hello-csharp
cd hello-csharp
```

The folder will look similar to this:

```text
hello-csharp/
  Program.cs
  hello-csharp.csproj
```

`Program.cs` is your C# source file.

`hello-csharp.csproj` is the project file. It tells .NET how to build the app.

## The Modern Template

Open `Program.cs`. Modern .NET templates usually create something like this:

```csharp
Console.WriteLine("Hello, World!");
```

That is a complete program.

It looks almost too small because C# now supports top-level statements. A
top-level statement is code written directly in the file, without manually
writing a `Program` class and `Main` method.

Read it as:

```text
When this program starts, write one line to the console.
```

Change it to:

```csharp
Console.WriteLine("Hello, C#");
```

Run:

```bash
dotnet run
```

Expected output:

```text
Hello, C#
```

## What `Console.WriteLine` Does

`Console` is a .NET type for terminal input and output.

`WriteLine` prints text and then moves to the next line.

```text
"Hello, C#" -> Console.WriteLine -> terminal output
```

This:

```csharp
Console.WriteLine("Name: Ada");
Console.WriteLine("Language: C#");
```

prints:

```text
Name: Ada
Language: C#
```

## The Classic `Main` Version

You will also see this style in older tutorials, books, and existing projects:

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

This is the same basic program written in the older explicit shape.

The important parts:

- `using System;` imports common framework names.
- `class Program` defines a type named `Program`.
- `Main` is the entry point where the program starts.
- `Console.WriteLine` prints text.

Modern top-level code is roughly a friendlier shortcut for this shape.

## Which Style Should You Use?

For beginner console apps in this course, use the modern template unless a
lesson says otherwise:

```csharp
Console.WriteLine("Hello, C#");
```

But you should be able to recognize the classic `Main` version because real C#
codebases contain both styles.

## The Project File

Open the `.csproj` file. It may look like this:

```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>net10.0</TargetFramework>
    <ImplicitUsings>enable</ImplicitUsings>
    <Nullable>enable</Nullable>
  </PropertyGroup>

</Project>
```

Your target framework may be different, such as `net8.0`, `net9.0`, or
`net10.0`.

The key ideas:

- `OutputType` says this project builds an executable app.
- `TargetFramework` says which .NET version the project targets.
- `ImplicitUsings` lets the template use common namespaces automatically.
- `Nullable` enables helpful warnings around possible `null` values.

You do not need to memorize this file yet. Just know it exists and controls how
the project builds.

## Common Beginner Mistakes

### Running The Command In The Wrong Folder

If `dotnet run` says it cannot find a project, check your folder:

```bash
pwd
ls
```

PowerShell:

```powershell
Get-Location
dir
```

You should be inside the folder that contains the `.csproj` file.

### Editing One Project And Running Another

If your output does not change, make sure your terminal is inside the same
project folder as the `Program.cs` file you edited.

### Confusing Strings And Code

This is text:

```csharp
"Hello, C#"
```

This is code:

```csharp
Console.WriteLine(...)
```

Quotes matter. If you remove them, C# will look for a variable or symbol with
that name.

## Practice

Change `Program.cs` to print:

```text
Name: Ada
Track: C#
Goal: Build useful software with .NET
```

Then replace the name and goal with your own.

Run after each small edit:

```bash
dotnet run
```

The habit is simple:

```text
edit -> save -> run -> read output
```

---

[**Next ->** Project Commands: new, build, run](./03-project-commands-new-build-run.md)  
[**<- Previous** Installing the .NET SDK](./01-installing-the-dotnet-sdk.md)
