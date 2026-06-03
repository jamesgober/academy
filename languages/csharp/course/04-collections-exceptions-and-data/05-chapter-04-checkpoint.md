<h1 align="center">
    <img width="99" alt="C# logo" src="../../../../_assets/logos/cs.svg">
    <br>
    <b>C#</b>
</h1>

[Home](../../../../README.md) / [C#](../../README.md) / [Chapter 04](./README.md)

---

# Chapter 04 Checkpoint

This checkpoint confirms your data and failure-handling patterns before async
and testing.

You will build a small inventory loader that:

- reads JSON
- deserializes records
- filters active items with LINQ
- groups data with a dictionary
- handles missing or invalid files clearly

## Project Shape

Create a console project:

```bash
dotnet new console -n inventory-loader
cd inventory-loader
```

Create a file named `inventory.json` beside the `.csproj`:

```json
[
  { "sku": "KB-100", "name": "Keyboard", "quantity": 12, "active": true },
  { "sku": "MS-200", "name": "Mouse", "quantity": 0, "active": true },
  { "sku": "HD-300", "name": "Old hard drive", "quantity": 4, "active": false }
]
```

Run this project from the project folder:

```bash
dotnet run
```

This matters because `path = "inventory.json"` is a relative path. The program
looks for the file in the current working directory.

## Step 1: Model The Data

Use a record for data loaded from JSON:

```csharp
public sealed record InventoryItem(
    string Sku,
    string Name,
    int Quantity,
    bool Active);
```

This is a good record use case because the JSON row is mostly data.

## Step 2: Read And Parse JSON

```csharp
using System.Text.Json;

string path = "inventory.json";

try
{
    string json = File.ReadAllText(path);
    List<InventoryItem>? items = JsonSerializer.Deserialize<List<InventoryItem>>(
        json,
        new JsonSerializerOptions
        {
            PropertyNameCaseInsensitive = true
        });

    if (items is null)
    {
        Console.WriteLine("Inventory file did not contain a valid item list.");
        return;
    }

    PrintReport(items);
}
catch (FileNotFoundException)
{
    Console.WriteLine($"Could not find inventory file: {path}");
}
catch (JsonException ex)
{
    Console.WriteLine($"Inventory JSON is invalid: {ex.Message}");
}
```

Important ideas:

- file I/O can fail
- JSON parsing can fail
- deserialization can return `null`
- the user should get a clear message

## Step 3: Filter With LINQ

```csharp
List<InventoryItem> activeItems = items
    .Where(item => item.Active)
    .ToList();
```

Read it as:

```text
from all items, keep only items where Active is true
```

## Step 4: Create A Dictionary For Lookup

```csharp
Dictionary<string, InventoryItem> bySku = activeItems
    .ToDictionary(item => item.Sku);
```

A dictionary is useful when you want fast lookup by key.

```csharp
if (bySku.TryGetValue("KB-100", out InventoryItem? keyboard))
{
    Console.WriteLine($"Lookup KB-100: {keyboard.Name}");
}
```

Use `TryGetValue` when a missing key is normal and should not crash the program.

## Step 5: Print A Report

```csharp
void PrintReport(List<InventoryItem> items)
{
    List<InventoryItem> activeItems = items
        .Where(item => item.Active)
        .ToList();

    Dictionary<string, InventoryItem> bySku = activeItems
        .ToDictionary(item => item.Sku);

    Console.WriteLine("Inventory report");
    Console.WriteLine("----------------");
    Console.WriteLine($"Total rows: {items.Count}");
    Console.WriteLine($"Active rows: {activeItems.Count}");

    foreach (InventoryItem item in activeItems.OrderBy(item => item.Sku))
    {
        string stockLabel = item.Quantity > 0 ? "in stock" : "out of stock";
        Console.WriteLine($"{item.Sku}: {item.Name} ({stockLabel})");
    }

    if (bySku.TryGetValue("KB-100", out InventoryItem? keyboard))
    {
        Console.WriteLine($"Lookup KB-100: {keyboard.Name}");
    }
}
```

## Complete Version

```csharp
using System.Text.Json;

string path = "inventory.json";

try
{
    string json = File.ReadAllText(path);
    List<InventoryItem>? items = JsonSerializer.Deserialize<List<InventoryItem>>(
        json,
        new JsonSerializerOptions
        {
            PropertyNameCaseInsensitive = true
        });

    if (items is null)
    {
        Console.WriteLine("Inventory file did not contain a valid item list.");
        return;
    }

    PrintReport(items);
}
catch (FileNotFoundException)
{
    Console.WriteLine($"Could not find inventory file: {path}");
}
catch (JsonException ex)
{
    Console.WriteLine($"Inventory JSON is invalid: {ex.Message}");
}

void PrintReport(List<InventoryItem> items)
{
    List<InventoryItem> activeItems = items
        .Where(item => item.Active)
        .ToList();

    Dictionary<string, InventoryItem> bySku = activeItems
        .ToDictionary(item => item.Sku);

    Console.WriteLine("Inventory report");
    Console.WriteLine("----------------");
    Console.WriteLine($"Total rows: {items.Count}");
    Console.WriteLine($"Active rows: {activeItems.Count}");

    foreach (InventoryItem item in activeItems.OrderBy(item => item.Sku))
    {
        string stockLabel = item.Quantity > 0 ? "in stock" : "out of stock";
        Console.WriteLine($"{item.Sku}: {item.Name} ({stockLabel})");
    }

    if (bySku.TryGetValue("KB-100", out InventoryItem? keyboard))
    {
        Console.WriteLine($"Lookup KB-100: {keyboard.Name}");
    }
}

public sealed record InventoryItem(
    string Sku,
    string Name,
    int Quantity,
    bool Active);
```

## Must-Be-Able Checklist

You are ready for Chapter 05 when you can explain:

- why `InventoryItem` is a record
- why deserialization returns a nullable value
- why `try`/`catch` belongs around file and JSON work
- what `Where` does
- why `ToList` materializes the filtered result
- when a dictionary is better than a list
- why `TryGetValue` is safer than indexing for optional lookups
- how the program handles missing files
- how the program handles invalid JSON

## Stretch Practice

Make one change at a time:

- Print only active items with quantity above zero.
- Add a `Category` property and group items by category.
- Detect duplicate SKUs before calling `ToDictionary`.
- Write a clean message when the inventory file is empty.
- Move report formatting into an `InventoryReporter` class.

---

[**Next ->** Async, Testing, and Capstone](../05-async-testing-and-capstone/README.md)  
[**<- Previous** Files and JSON Basics](./04-files-and-json-basics.md)
