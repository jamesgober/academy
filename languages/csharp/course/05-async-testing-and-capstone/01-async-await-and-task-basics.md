<h1 align="center">
    <img width="99" alt="C# logo" src="../../../../_assets/logos/cs.svg">
    <br>
    <b>C#</b>
</h1>

[Home](../../../../README.md) / [C#](../../README.md) / [Chapter 05](./README.md)

---

# Async/Await And Task Basics

> Async code is for waiting efficiently. It does not make CPU work magically
> faster. It lets your program do useful work while file, network, database, or
> timer operations are waiting.

**You will learn:**
- What `Task` represents
- What `async` changes
- What `await` does
- Why `.Result` and `.Wait()` are risky
- How async flows through method calls
- How cancellation works at a beginner level
- How to write a practical async file example

**Before this page, you should know:** [Exception Handling And Failure Design](../04-collections-exceptions-and-data/02-exception-handling-and-failure-design.md)

---

## The Beginner Mental Model

Synchronous code:

```text
start operation
wait here doing nothing
continue after result arrives
```

Async code:

```text
start operation
give control back while waiting
continue here when result arrives
```

Async is especially useful for:

- reading files
- writing files
- HTTP requests
- database calls
- timers
- UI apps that must stay responsive
- servers that must handle many requests

Async is usually not useful for:

- simple math
- tiny CPU-only loops
- code that already has the answer in memory

---

## What Is `Task`?

A `Task` represents work that may finish later.

```csharp
Task waitTask = Task.Delay(1000);
```

A `Task<T>` represents work that may produce a value later.

```csharp
Task<string> readTask = File.ReadAllTextAsync("notes.txt");
```

Plain language:

```text
Task<string> means: I will eventually have a string, unless the work fails or is canceled.
```

---

## What `async` And `await` Do

```csharp
public async Task<string> LoadTextAsync(string path)
{
    string content = await File.ReadAllTextAsync(path);
    return content;
}
```

`async` allows the method to use `await`.

`await` means:

```text
Pause this method until the task completes.
Do not block the thread while waiting.
Resume here with the result.
```

The method returns `Task<string>` because the string is not available
immediately.

---

## Async Flows Upward

If one method awaits, its caller often needs to await too.

```csharp
public async Task PrintFileAsync(string path)
{
    string text = await LoadTextAsync(path);
    Console.WriteLine(text);
}
```

Then:

```csharp
await PrintFileAsync("notes.txt");
```

Modern C# allows async `Main`:

```csharp
public static async Task Main()
{
    await PrintFileAsync("notes.txt");
}
```

---

## Avoid `.Result` And `.Wait()`

Avoid:

```csharp
string text = LoadTextAsync("notes.txt").Result;
```

Avoid:

```csharp
LoadTextAsync("notes.txt").Wait();
```

Problems:

- blocks the current thread
- can reduce scalability
- can cause deadlocks in some app models
- wraps exceptions awkwardly in some cases

Beginner rule:

```text
Use await all the way up when possible.
```

---

## Exception Handling With Async

Use normal `try` / `catch` around `await`.

```csharp
public async Task<string?> TryLoadTextAsync(string path)
{
    try
    {
        return await File.ReadAllTextAsync(path);
    }
    catch (FileNotFoundException)
    {
        return null;
    }
}
```

The exception is observed when you await the task.

---

## Cancellation

Long async operations should often accept a `CancellationToken`.

```csharp
public async Task<string> LoadTextAsync(
    string path,
    CancellationToken cancellationToken
)
{
    return await File.ReadAllTextAsync(path, cancellationToken);
}
```

Callers can request cancellation:

```csharp
using var cts = new CancellationTokenSource();

Task<string> task = LoadTextAsync("notes.txt", cts.Token);

cts.Cancel();
```

Cancellation is cooperative:

```text
The caller requests cancellation.
The async operation checks the token.
The operation stops if it supports cancellation.
```

---

## `Task.WhenAll`

Use `Task.WhenAll` to wait for multiple independent async operations.

```csharp
public async Task PrintBothAsync()
{
    Task<string> firstTask = File.ReadAllTextAsync("first.txt");
    Task<string> secondTask = File.ReadAllTextAsync("second.txt");

    string[] results = await Task.WhenAll(firstTask, secondTask);

    foreach (string text in results)
    {
        Console.WriteLine(text);
    }
}
```

This starts both reads before waiting for both results.

Notice:

```text
Use WhenAll for independent work.
Do not use it when one result is needed before starting the next operation.
```

---

## Real Example: Async Notes Store

```csharp
using System.Text.Json;

public sealed class NotesStore
{
    private readonly string _path;

    public NotesStore(string path)
    {
        _path = path;
    }

    public async Task<List<string>> LoadAsync(
        CancellationToken cancellationToken = default
    )
    {
        if (!File.Exists(_path))
        {
            return new List<string>();
        }

        string json = await File.ReadAllTextAsync(_path, cancellationToken);

        return JsonSerializer.Deserialize<List<string>>(json)
            ?? new List<string>();
    }

    public async Task SaveAsync(
        IReadOnlyCollection<string> notes,
        CancellationToken cancellationToken = default
    )
    {
        string json = JsonSerializer.Serialize(
            notes,
            new JsonSerializerOptions { WriteIndented = true }
        );

        await File.WriteAllTextAsync(_path, json, cancellationToken);
    }
}
```

Use:

```csharp
var store = new NotesStore("notes.json");

List<string> notes = await store.LoadAsync();
notes.Add("Learn async");
await store.SaveAsync(notes);
```

---

## Common Mistakes

### Mistake 1: Async Without Await

```csharp
public async Task SaveAsync()
{
    File.WriteAllTextAsync("notes.txt", "hello");
}
```

This starts work but does not await it.

Better:

```csharp
public async Task SaveAsync()
{
    await File.WriteAllTextAsync("notes.txt", "hello");
}
```

### Mistake 2: `async void`

Avoid:

```csharp
public async void Save()
{
    await File.WriteAllTextAsync("notes.txt", "hello");
}
```

Use:

```csharp
public async Task SaveAsync()
{
    await File.WriteAllTextAsync("notes.txt", "hello");
}
```

`async void` is mainly for event handlers.

### Mistake 3: Pretending Async Makes CPU Work Faster

This does not need async:

```csharp
public async Task<int> AddAsync(int a, int b)
{
    return a + b;
}
```

If the work is immediate CPU work, keep it synchronous.

---

## Mini Project: Async File Counter

Build:

```csharp
public static async Task<int> CountLinesAsync(string path)
{
    if (!File.Exists(path))
    {
        return 0;
    }

    string[] lines = await File.ReadAllLinesAsync(path);
    return lines.Length;
}
```

Then call:

```csharp
int count = await CountLinesAsync("notes.txt");
Console.WriteLine($"Lines: {count}");
```

Stretch:

- add `CancellationToken`
- catch `UnauthorizedAccessException`
- count lines in multiple files with `Task.WhenAll`

---

## Chapter Checkpoint

You should now be able to answer:

- What does `Task` represent?
- What does `Task<T>` represent?
- What does `await` do?
- Why should you avoid `.Result` and `.Wait()` in app code?
- Why does async often flow upward?
- How does cancellation work?
- When should you use `Task.WhenAll`?
- Why is async not for simple CPU math?

---

## Recap

- Async code waits efficiently.
- `Task` represents work that completes later.
- `await` pauses the method without blocking the thread.
- Exceptions are handled around `await`.
- Cancellation uses `CancellationToken`.
- `Task.WhenAll` waits for independent async operations.
- Prefer `Task` over `async void`.

## Try It Yourself

Convert one synchronous file operation to async:

- read file
- parse lines
- print a count
- handle missing file
- avoid `.Result` and `.Wait()`

---

[**Next ->** Unit Testing With xUnit](./02-unit-testing-with-xunit.md)  
[**<- Previous** Chapter Start](./README.md)
