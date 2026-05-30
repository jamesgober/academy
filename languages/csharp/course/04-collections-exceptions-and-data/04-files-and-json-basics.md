<h1 align="center">
    <img width="99" alt="C# logo" src="../../../../_assets/logos/cs.svg">
    <br>
    <b>C#</b>
</h1>

[Home](../../../../README.md) / [C#](../../README.md) / [Chapter 04](./README.md)

---

# Files And JSON Basics

> Real applications keep data somewhere. For beginner C#, text files and JSON
> are the easiest way to learn persistence without needing a database yet.

**You will learn:**
- How to read and write text files
- How paths work at a beginner level
- How to serialize objects to JSON
- How to deserialize JSON safely
- How to handle missing or malformed files
- Why DTOs can keep file formats simple

**Before this page, you should know:** [LINQ Query Patterns](./03-linq-query-patterns.md)

---

## Text File Write

```csharp
await File.WriteAllTextAsync("note.txt", "Hello data");
```

This creates or overwrites `note.txt`.

Risk notice:

```text
WriteAllTextAsync replaces the file contents. If you need to add to a file, use
AppendAllTextAsync.
```

---

## Text File Read

```csharp
string text = await File.ReadAllTextAsync("note.txt");
Console.WriteLine(text);
```

If the file does not exist, this throws `FileNotFoundException`.

Safer:

```csharp
if (File.Exists("note.txt"))
{
    string text = await File.ReadAllTextAsync("note.txt");
    Console.WriteLine(text);
}
else
{
    Console.WriteLine("No note saved yet.");
}
```

---

## Paths

Relative path:

```csharp
"orders.json"
```

means:

```text
orders.json in the program's current working directory.
```

Build a path:

```csharp
string path = Path.Combine("data", "orders.json");
```

Create directory if needed:

```csharp
Directory.CreateDirectory("data");
```

---

## JSON Serialize

```csharp
using System.Text.Json;

public sealed record AppSettings(string Environment, int Retries);

var settings = new AppSettings("prod", 3);

string json = JsonSerializer.Serialize(
    settings,
    new JsonSerializerOptions { WriteIndented = true }
);

await File.WriteAllTextAsync("settings.json", json);
```

Output:

```json
{
  "Environment": "prod",
  "Retries": 3
}
```

---

## JSON Deserialize

```csharp
string json = await File.ReadAllTextAsync("settings.json");

AppSettings? settings = JsonSerializer.Deserialize<AppSettings>(json);

if (settings is null)
{
    Console.WriteLine("Settings file was empty or invalid.");
}
else
{
    Console.WriteLine(settings.Environment);
}
```

Important:

```text
Deserialize can return null. Check it.
```

---

## Handle Bad JSON

```csharp
try
{
    string json = await File.ReadAllTextAsync("settings.json");
    AppSettings? settings = JsonSerializer.Deserialize<AppSettings>(json);

    if (settings is null)
    {
        Console.WriteLine("Settings were missing.");
    }
}
catch (JsonException)
{
    Console.WriteLine("Settings file is not valid JSON.");
}
catch (IOException ex)
{
    Console.WriteLine($"Could not read settings: {ex.Message}");
}
```

Catch the failures you know how to explain.

---

## DTOs

DTO means Data Transfer Object.

Use DTOs when the file shape should be simpler than the domain model.

```csharp
public sealed record OrderDto(
    int Id,
    List<OrderItemDto> Items
);

public sealed record OrderItemDto(
    string Sku,
    int Quantity,
    decimal UnitPrice
);
```

DTOs are useful for:

- JSON files
- API request/response bodies
- database transfer shapes
- keeping domain classes private and rule-focused

---

## Real Example: Settings Store

```csharp
using System.Text.Json;

public sealed record AppSettings(string Environment, int Retries);

public sealed class SettingsStore
{
    private readonly string _path;
    private readonly JsonSerializerOptions _options = new()
    {
        WriteIndented = true
    };

    public SettingsStore(string path)
    {
        _path = path;
    }

    public async Task<AppSettings> LoadAsync()
    {
        if (!File.Exists(_path))
        {
            return new AppSettings("dev", 3);
        }

        string json = await File.ReadAllTextAsync(_path);

        return JsonSerializer.Deserialize<AppSettings>(json, _options)
            ?? new AppSettings("dev", 3);
    }

    public async Task SaveAsync(AppSettings settings)
    {
        string? directory = Path.GetDirectoryName(_path);

        if (!string.IsNullOrWhiteSpace(directory))
        {
            Directory.CreateDirectory(directory);
        }

        string json = JsonSerializer.Serialize(settings, _options);
        await File.WriteAllTextAsync(_path, json);
    }
}
```

Use:

```csharp
var store = new SettingsStore(Path.Combine("data", "settings.json"));

AppSettings settings = await store.LoadAsync();
Console.WriteLine(settings.Environment);

await store.SaveAsync(settings with { Retries = 5 });
```

---

## Common Mistakes

### Mistake 1: Assuming The File Exists

Always decide what missing file means.

For settings, it might mean default settings.

For required import, it might mean show an error.

### Mistake 2: Ignoring Null From Deserialize

Always handle `null`.

### Mistake 3: Saving Domain Objects Too Early

If your domain object protects private state, direct serialization may be awkward.
Use DTOs when needed.

---

## Chapter Checkpoint

You should now be able to answer:

- What does `WriteAllTextAsync` do?
- What happens when `ReadAllTextAsync` reads a missing file?
- What is a relative path?
- What does `JsonSerializer.Serialize` do?
- Why can `Deserialize` return null?
- What is a DTO?
- When should you catch `JsonException`?

---

## Recap

- Files let beginner apps persist data.
- Relative paths depend on current working directory.
- JSON is a practical beginner persistence format.
- Missing files and malformed JSON need explicit behavior.
- DTOs keep file shapes simple.

## Try It Yourself

Serialize one object to JSON, save it to disk, read it back, deserialize it, and
handle:

- missing file
- invalid JSON
- null result

---

[**Next ->** Chapter 04 Checkpoint](./05-chapter-04-checkpoint.md)  
[**<- Previous** LINQ Query Patterns](./03-linq-query-patterns.md)
