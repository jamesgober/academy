<h1 align="center">
    <img width="99" alt="C# logo" src="../../../../_assets/logos/cs.svg">
    <br>
    <b>C#</b>
</h1>

[Home](../../../../README.md) / [C#](../../README.md) / [Chapter 01](./README.md)

---

# Chapter 01 Checkpoint

This checkpoint proves your .NET toolchain works and that you understand the
basic project workflow.

You are ready for Chapter 02 only when you can create, edit, build, run, break,
and fix a small console project without guessing.

## Goal

Build a tiny console app that prints a learner profile:

```text
C# checkpoint
-------------
Name: Ada
Goal: Build useful .NET apps
Status: ready
```

## Step 1: Create The Project

Run:

```bash
dotnet new console -n checkpoint-one
cd checkpoint-one
```

Confirm the files:

```bash
ls
```

PowerShell:

```powershell
dir
```

You should see a `.csproj` file and `Program.cs`.

## Step 2: Edit `Program.cs`

Replace the generated code with:

```csharp
Console.WriteLine("C# checkpoint");
Console.WriteLine("-------------");
Console.WriteLine("Name: Ada");
Console.WriteLine("Goal: Build useful .NET apps");
Console.WriteLine("Status: ready");
```

Change `Ada` and the goal if you want.

## Step 3: Build

Run:

```bash
dotnet build
```

If it succeeds, continue.

If it fails, read the first error. Find:

```text
file
line
column
diagnostic ID
message
```

Example diagnostic IDs look like `CS1002` or `CS0103`.

## Step 4: Run

Run:

```bash
dotnet run
```

Confirm the output matches your `Program.cs`.

## Step 5: Break It On Purpose

Remove the semicolon from one line:

```csharp
Console.WriteLine("Status: ready")
```

Build again:

```bash
dotnet build
```

Find the first compiler error and write down:

```text
diagnostic ID:
file:
line:
message:
what I think it means:
```

Then put the semicolon back and build again.

## Step 6: Prove You Know The Commands

Run:

```bash
dotnet --list-sdks
dotnet clean
dotnet build
dotnet run
```

Explain each command in your own words.

## Must-Be-Able Checklist

You are ready for Chapter 02 when you can:

- create a console project with `dotnet new console`
- identify `Program.cs`
- identify the `.csproj` file
- explain what `dotnet build` does
- explain what `dotnet run` does
- explain why `bin/` and `obj/` appear
- read a C# compiler diagnostic ID
- fix a missing semicolon error
- run the same project you edited

## Stretch Practice

Add one more line:

```text
Next skill: variables
```

Then intentionally misspell `Console` as `Consol`. Build, read the first error,
fix it, and build again.

The point is not to avoid errors. The point is to become comfortable using the
toolchain to find and fix them.

---

[**Next ->** Core Language Basics](../02-core-language-basics/README.md)  
[**<- Previous** Reading Errors and Warnings](./04-reading-errors-and-warnings.md)
