<h1 align="center">
    <img width="99" alt="C# logo" src="../../../../_assets/logos/cs.svg">
    <br>
    <b>C#</b>
</h1>

[Home](../../../../README.md) / [C#](../../README.md) / [Chapter 03](./README.md)

---

# Chapter 03 Checkpoint

This checkpoint validates your object-modeling decisions before data-heavy
programming begins.

You will model a tiny invoice domain with:

- a class that protects its state
- a record for simple data
- an interface for formatting
- a concrete formatter implementation
- a short explanation of each type choice

## Program Goal

Build enough code to print:

```text
Invoice INV-1001
Customer: Ada Lovelace
Lines:
- Keyboard x 1 @ $89.99 = $89.99
- Mouse x 2 @ $24.50 = $49.00
Total: $138.99
```

## Type Design

Use these choices:

```text
Invoice            class   owns behavior and protects a list of lines
InvoiceLine        record  simple immutable line-item data
IInvoiceFormatter  interface  contract for output formatting
ConsoleFormatter   class   concrete behavior that prints an invoice
```

The key idea: do not make everything a class just because C# has classes.
Choose the type shape that matches the job.

## Step 1: Create The Line Item Record

```csharp
public sealed record InvoiceLine(string Description, int Quantity, decimal UnitPrice)
{
    public decimal LineTotal => Quantity * UnitPrice;
}
```

Why a record?

- It is mostly data.
- It has value-style equality.
- It is easy to create and read.
- The properties are immutable by default in this positional shape.

`LineTotal` is a calculated property. It is not stored separately because it can
always be calculated from quantity and unit price.

## Step 2: Create The Invoice Class

```csharp
public sealed class Invoice
{
    private readonly List<InvoiceLine> _lines = [];

    public Invoice(string number, string customerName)
    {
        if (string.IsNullOrWhiteSpace(number))
        {
            throw new ArgumentException("Invoice number is required.", nameof(number));
        }

        if (string.IsNullOrWhiteSpace(customerName))
        {
            throw new ArgumentException("Customer name is required.", nameof(customerName));
        }

        Number = number;
        CustomerName = customerName;
    }

    public string Number { get; }
    public string CustomerName { get; }
    public IReadOnlyList<InvoiceLine> Lines => _lines;
    public decimal Total => _lines.Sum(line => line.LineTotal);

    public void AddLine(string description, int quantity, decimal unitPrice)
    {
        if (string.IsNullOrWhiteSpace(description))
        {
            throw new ArgumentException("Description is required.", nameof(description));
        }

        if (quantity <= 0)
        {
            throw new ArgumentOutOfRangeException(nameof(quantity), "Quantity must be positive.");
        }

        if (unitPrice < 0m)
        {
            throw new ArgumentOutOfRangeException(nameof(unitPrice), "Unit price cannot be negative.");
        }

        _lines.Add(new InvoiceLine(description, quantity, unitPrice));
    }
}
```

Why a class?

- It owns a private list.
- It enforces rules before lines are added.
- It exposes read-only views to callers.
- It has behavior, not just data.

The private `_lines` list is mutable inside the class. Outside code only sees
`IReadOnlyList<InvoiceLine>`, so callers cannot casually corrupt the invoice.

## Step 3: Create The Formatter Interface

```csharp
public interface IInvoiceFormatter
{
    void Print(Invoice invoice);
}
```

An interface is a contract. It says what a formatter must do without saying how
it does it.

Later, you could create:

```text
ConsoleInvoiceFormatter
JsonInvoiceFormatter
HtmlInvoiceFormatter
```

All of them could follow the same `IInvoiceFormatter` contract.

## Step 4: Implement A Console Formatter

```csharp
public sealed class ConsoleInvoiceFormatter : IInvoiceFormatter
{
    public void Print(Invoice invoice)
    {
        Console.WriteLine($"Invoice {invoice.Number}");
        Console.WriteLine($"Customer: {invoice.CustomerName}");
        Console.WriteLine("Lines:");

        foreach (InvoiceLine line in invoice.Lines)
        {
            Console.WriteLine(
                $"- {line.Description} x {line.Quantity} @ {line.UnitPrice:C} = {line.LineTotal:C}");
        }

        Console.WriteLine($"Total: {invoice.Total:C}");
    }
}
```

This class has behavior, so `class` is a natural fit.

## Complete Demo

```csharp
Invoice invoice = new("INV-1001", "Ada Lovelace");
invoice.AddLine("Keyboard", 1, 89.99m);
invoice.AddLine("Mouse", 2, 24.50m);

IInvoiceFormatter formatter = new ConsoleInvoiceFormatter();
formatter.Print(invoice);

public sealed record InvoiceLine(string Description, int Quantity, decimal UnitPrice)
{
    public decimal LineTotal => Quantity * UnitPrice;
}

public sealed class Invoice
{
    private readonly List<InvoiceLine> _lines = [];

    public Invoice(string number, string customerName)
    {
        if (string.IsNullOrWhiteSpace(number))
        {
            throw new ArgumentException("Invoice number is required.", nameof(number));
        }

        if (string.IsNullOrWhiteSpace(customerName))
        {
            throw new ArgumentException("Customer name is required.", nameof(customerName));
        }

        Number = number;
        CustomerName = customerName;
    }

    public string Number { get; }
    public string CustomerName { get; }
    public IReadOnlyList<InvoiceLine> Lines => _lines;
    public decimal Total => _lines.Sum(line => line.LineTotal);

    public void AddLine(string description, int quantity, decimal unitPrice)
    {
        if (string.IsNullOrWhiteSpace(description))
        {
            throw new ArgumentException("Description is required.", nameof(description));
        }

        if (quantity <= 0)
        {
            throw new ArgumentOutOfRangeException(nameof(quantity), "Quantity must be positive.");
        }

        if (unitPrice < 0m)
        {
            throw new ArgumentOutOfRangeException(nameof(unitPrice), "Unit price cannot be negative.");
        }

        _lines.Add(new InvoiceLine(description, quantity, unitPrice));
    }
}

public interface IInvoiceFormatter
{
    void Print(Invoice invoice);
}

public sealed class ConsoleInvoiceFormatter : IInvoiceFormatter
{
    public void Print(Invoice invoice)
    {
        Console.WriteLine($"Invoice {invoice.Number}");
        Console.WriteLine($"Customer: {invoice.CustomerName}");
        Console.WriteLine("Lines:");

        foreach (InvoiceLine line in invoice.Lines)
        {
            Console.WriteLine(
                $"- {line.Description} x {line.Quantity} @ {line.UnitPrice:C} = {line.LineTotal:C}");
        }

        Console.WriteLine($"Total: {invoice.Total:C}");
    }
}
```

## Must-Be-Able Checklist

You are ready for Chapter 04 when you can explain:

- why `Invoice` is a class
- why `InvoiceLine` is a record
- why `_lines` is private
- why `Lines` returns `IReadOnlyList<InvoiceLine>`
- why constructors are good places to enforce required state
- why `AddLine` validates inputs
- what `IInvoiceFormatter` promises
- why interface variables can hold concrete implementations
- how `Total` is calculated from the line items

## Stretch Practice

Improve one piece at a time:

- Add a `RemoveLine` method.
- Add a `TaxRate` property and calculate tax.
- Add a `PlainTextInvoiceFormatter` that returns a string instead of printing.
- Add a fake formatter for tests.
- Add a rule that an invoice cannot be printed with zero lines.

---

[**Next ->** Collections, Exceptions, and Data](../04-collections-exceptions-and-data/README.md)  
[**<- Previous** Records, Structs, and When to Use Them](./04-records-structs-and-when-to-use-them.md)
