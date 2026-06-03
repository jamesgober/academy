<h1 align="center">
    <img width="99" alt="C# logo" src="../../../../_assets/logos/cs.svg">
    <br>
    <b>C#</b>
</h1>

[Home](../../../../README.md) / [C#](../../README.md) / [Chapter 02](./README.md)

---

# Chapter 02 Checkpoint

This checkpoint confirms your core C# syntax is strong before object-oriented
design begins.

You will build a small order-total reporter. It uses:

- `decimal` for money
- string interpolation
- methods with return values
- `if` validation
- switch expressions
- `foreach` loops
- accumulation

## Program Goal

Expected output:

```text
Order totals
------------
Order 1001: $24.99 -> Small
Order 1002: $149.50 -> Medium
Order 1003: $220.00 -> Large
Grand total: $394.49
Average: $131.50
```

## Step 1: Create The Project

```bash
dotnet new console -n order-total-checkpoint
cd order-total-checkpoint
```

Replace `Program.cs` with:

```csharp
decimal[] totals = [24.99m, 149.50m, 220.00m];

Console.WriteLine("Order totals");
Console.WriteLine("------------");
```

`decimal` is the right beginner choice for money because it avoids many
floating-point rounding surprises.

The `m` suffix means "this number is a decimal literal."

## Step 2: Describe One Total

Add a local function:

```csharp
string DescribeOrderTotal(decimal total)
{
    return total switch
    {
        < 0m => "Invalid",
        < 50m => "Small",
        < 200m => "Medium",
        _ => "Large"
    };
}
```

Read the switch expression like this:

```text
if total is below 0      -> Invalid
else if total is below 50 -> Small
else if total is below 200 -> Medium
otherwise                -> Large
```

Order matters. The first matching arm wins.

## Step 3: Loop Through The Orders

Add:

```csharp
for (int index = 0; index < totals.Length; index++)
{
    int orderNumber = 1001 + index;
    decimal total = totals[index];
    string label = DescribeOrderTotal(total);

    Console.WriteLine($"Order {orderNumber}: {total:C} -> {label}");
}
```

Important details:

- arrays start at index `0`
- `totals.Length` is the number of items
- `{total:C}` formats the decimal as currency
- `orderNumber` is separate from the array index so the output reads naturally

## Step 4: Calculate A Grand Total

Add a method-like local function:

```csharp
decimal CalculateGrandTotal(decimal[] values)
{
    decimal grandTotal = 0m;

    foreach (decimal value in values)
    {
        grandTotal += value;
    }

    return grandTotal;
}
```

This is accumulation:

```text
start at 0
add first value
add second value
add third value
return the result
```

Use it:

```csharp
decimal grandTotal = CalculateGrandTotal(totals);
Console.WriteLine($"Grand total: {grandTotal:C}");
```

## Step 5: Calculate An Average Safely

Add:

```csharp
decimal CalculateAverage(decimal[] values)
{
    if (values.Length == 0)
    {
        return 0m;
    }

    return CalculateGrandTotal(values) / values.Length;
}
```

The `if` prevents dividing by zero.

Use it:

```csharp
decimal average = CalculateAverage(totals);
Console.WriteLine($"Average: {average:C}");
```

## Complete Version

```csharp
decimal[] totals = [24.99m, 149.50m, 220.00m];

Console.WriteLine("Order totals");
Console.WriteLine("------------");

for (int index = 0; index < totals.Length; index++)
{
    int orderNumber = 1001 + index;
    decimal total = totals[index];
    string label = DescribeOrderTotal(total);

    Console.WriteLine($"Order {orderNumber}: {total:C} -> {label}");
}

decimal grandTotal = CalculateGrandTotal(totals);
decimal average = CalculateAverage(totals);

Console.WriteLine($"Grand total: {grandTotal:C}");
Console.WriteLine($"Average: {average:C}");

string DescribeOrderTotal(decimal total)
{
    return total switch
    {
        < 0m => "Invalid",
        < 50m => "Small",
        < 200m => "Medium",
        _ => "Large"
    };
}

decimal CalculateGrandTotal(decimal[] values)
{
    decimal grandTotal = 0m;

    foreach (decimal value in values)
    {
        grandTotal += value;
    }

    return grandTotal;
}

decimal CalculateAverage(decimal[] values)
{
    if (values.Length == 0)
    {
        return 0m;
    }

    return CalculateGrandTotal(values) / values.Length;
}
```

## Must-Be-Able Checklist

You are ready for Chapter 03 when you can explain:

- why money uses `decimal`
- why decimal literals use `m`
- how a `for` loop uses an index
- when `foreach` is simpler than `for`
- what a method receives as parameters
- what a method returns
- how switch expression arms are matched
- why empty input needs a guard clause
- how string interpolation works

## Stretch Practice

Make one change at a time:

- Add a fourth order total.
- Add a `Very Large` label for totals `500` and above.
- Count how many orders are `Large`.
- Print `No orders available` when the array is empty.
- Rewrite the order-printing loop with `foreach` and a separate order number.

---

[**Next ->** Classes and OOP](../03-classes-and-oop/README.md)  
[**<- Previous** Loops and Iteration Patterns](./04-loops-and-iteration-patterns.md)
