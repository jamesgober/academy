<h1 align="center">
    <img width="99" alt="C# logo" src="../../../../_assets/logos/cs.svg">
    <br>
    <b>C#</b>
</h1>

[Home](../../../../README.md) / [C#](../../README.md) / [Chapter 01](./README.md)

---

# Reading Errors And Warnings

Compiler output is not noise. It is a map from "something is wrong" to "here is
where to look first."

The skill is learning to read it calmly.

## A Typical Diagnostic

Example:

```text
Program.cs(12,17): error CS0103: The name 'totla' does not exist in the current context
```

Read it in this order:

```text
Program.cs      file
(12,17)         line 12, column 17
error           severity
CS0103          diagnostic ID
message         what the compiler thinks is wrong
```

The file and line tell you where to begin. The diagnostic ID gives you a stable
code you can search in docs or online.

## Errors Versus Warnings

An error stops the build:

```text
error CS1002: ; expected
```

A warning allows the build to continue, but points to risk:

```text
warning CS8602: Dereference of a possibly null reference.
```

Beginner rule:

```text
fix errors first, then warnings
```

Professional rule:

```text
do not treat warnings as decoration
```

Warnings often identify future runtime bugs.

## Common First Errors

### Missing Semicolon

```csharp
Console.WriteLine("Hello")
```

Likely diagnostic:

```text
CS1002: ; expected
```

Fix:

```csharp
Console.WriteLine("Hello");
```

### Misspelled Name

```csharp
decimal total = 10m;
Console.WriteLine(totla);
```

Likely diagnostic:

```text
CS0103: The name 'totla' does not exist in the current context
```

Fix the spelling:

```csharp
Console.WriteLine(total);
```

### Wrong Type

```csharp
int count = "five";
```

Likely diagnostic:

```text
CS0029: Cannot implicitly convert type 'string' to 'int'
```

Fix by using the right value type:

```csharp
int count = 5;
```

or the right variable type:

```csharp
string count = "five";
```

## Nullability Warnings

With nullable analysis enabled, C# warns when a value might be `null`.

Example:

```csharp
string? name = GetName();
Console.WriteLine(name.Length);
```

The compiler may warn:

```text
CS8602: Dereference of a possibly null reference.
```

The issue:

```text
name might be null
name.Length would crash if name is null
```

Fix with a check:

```csharp
string? name = GetName();

if (name is not null)
{
    Console.WriteLine(name.Length);
}
```

or a fallback:

```csharp
string displayName = name ?? "Unknown";
Console.WriteLine(displayName.Length);
```

## The Triage Loop

Use this process:

```text
1. Run dotnet build.
2. Read the first error.
3. Go to the file and line.
4. Fix the smallest likely cause.
5. Run dotnet build again.
6. Repeat until errors are gone.
7. Resolve warnings.
```

Do not try to fix twenty diagnostics at once. One syntax mistake can create many
downstream messages.

## Practice: Create And Fix `CS0103`

Start with:

```csharp
string learnerName = "Ada";
Console.WriteLine(learnerName);
```

Break it:

```csharp
string learnerName = "Ada";
Console.WriteLine(leanerName);
```

Build:

```bash
dotnet build
```

Find:

- file
- line
- diagnostic ID
- message

Fix the spelling and build again.

## Practice: Create And Fix A Nullability Warning

Use:

```csharp
string? nickname = null;
Console.WriteLine(nickname.Length);
```

Build and read the warning.

Then fix it:

```csharp
string? nickname = null;

if (nickname is not null)
{
    Console.WriteLine(nickname.Length);
}
else
{
    Console.WriteLine("No nickname");
}
```

## What You Should Be Able To Explain

Before the checkpoint, make sure you can explain:

- what `Program.cs(12,17)` means
- why diagnostic IDs are useful
- why the first error is the best starting point
- why warnings still matter
- what a nullability warning is trying to prevent
- why rebuilding after each fix is better than guessing

---

[**Next ->** Chapter 01 Checkpoint](./05-chapter-01-checkpoint.md)  
[**<- Previous** Project Commands: new, build, run](./03-project-commands-new-build-run.md)
