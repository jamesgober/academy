<h1 align="center">
    <img width="99" alt="C# logo" src="../../../../_assets/logos/cs.svg">
    <br>
    <b>C#</b>
</h1>

<!-- ===== HEAD NAV ===== -->
<div align="center">

[Home](../../../../README.md) · [C#](../../README.md) · [Chapter 04](./README.md)

</div>

---

# Files and JSON Basics

> Real applications read and write data, not just print output.

**You will learn:**
- Reading and writing text files
- Serializing objects with `System.Text.Json`
- Basic file and data validation habits

**Before this page, you should know:** classes and collections.

---

## File operations

```csharp
await File.WriteAllTextAsync("note.txt", "Hello data");
string text = await File.ReadAllTextAsync("note.txt");
```

## JSON operations

```csharp
var settings = new AppSettings("prod", 3);
string json = JsonSerializer.Serialize(settings);
var parsed = JsonSerializer.Deserialize<AppSettings>(json);

public record AppSettings(string Environment, int Retries);
```

Validation habit:
- check file existence before read
- guard against null from deserialization

---

## Recap

- File APIs are straightforward but still need validation.
- `System.Text.Json` handles most beginner and intermediate needs.
- Handle missing files and malformed data gracefully.

## Try it yourself

Serialize one object to JSON, save it to disk, read it back, and deserialize it.

---

<!-- ===== FOOT NAV ===== -->
<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← LINQ Query Patterns](./03-linq-query-patterns.md) | [Chapter](./README.md) · [Track](../../README.md) · [Home](../../../../README.md) | [Chapter 04 Checkpoint →](./05-chapter-04-checkpoint.md) |

</div>
