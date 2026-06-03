<h1 align="center">
    <img width="99" alt="C# logo" src="../../../../_assets/logos/cs.svg">
    <br>
    <b>C#</b>
</h1>

[Home](../../../../README.md) / [C#](../../README.md) / [Chapter 02](./README.md)

---

# Methods, Parameters, And Returns

> Methods let you name behavior. A good method says what it needs, what it
> returns, and what rule it protects.

**You will learn:**
- How method signatures work
- How parameters work
- How return values work
- When to use `out`
- Why `ref` should be rare
- How optional parameters work
- How overloads work
- How to design beginner-friendly methods

**Before this page, you should know:** [Types, Variables, And Strings](./01-types-variables-and-strings.md)

---

## Basic Method

```csharp
static int Add(int left, int right)
{
    return left + right;
}
```

Read:

```text
Add takes two ints.
Add returns an int.
```

Call:

```csharp
int total = Add(2, 3);
```

---

## Method Signature

```text
static int Add(int left, int right)
       |   |   |
       |   |   parameters
       |   name
       return type
```

`static` means this method belongs to the type/program, not an object instance.
You will use many `static` methods in early console examples.

---

## `void`

Use `void` when the method does not return a value.

```csharp
static void PrintError(string message)
{
    Console.WriteLine($"Error: {message}");
}
```

Call:

```csharp
PrintError("Invalid quantity");
```

---

## Return Values Are Usually Better Than Mutation

Prefer:

```csharp
static decimal ApplyDiscount(decimal price, decimal percent)
{
    return price - (price * percent);
}
```

Over:

```csharp
static void ApplyDiscount(ref decimal price, decimal percent)
{
    price = price - (price * percent);
}
```

Returning values is easier to read and test.

---

## `out` And Try Methods

`out` is common in the Try pattern.

```csharp
static bool TryReadPositiveInt(string? input, out int value)
{
    value = 0;

    if (!int.TryParse(input, out int parsed))
    {
        return false;
    }

    if (parsed <= 0)
    {
        return false;
    }

    value = parsed;
    return true;
}
```

Use:

```csharp
if (TryReadPositiveInt(Console.ReadLine(), out int quantity))
{
    Console.WriteLine($"Quantity: {quantity}");
}
else
{
    Console.WriteLine("Quantity must be positive.");
}
```

`out` means the method will assign the variable before returning.

---

## `ref`

`ref` lets a method modify the caller's variable.

```csharp
static void ClampToZero(ref int value)
{
    if (value < 0)
    {
        value = 0;
    }
}
```

Use:

```csharp
int stock = -5;
ClampToZero(ref stock);
```

Use `ref` rarely. Most beginner methods should return a value instead.

---

## Optional Parameters

```csharp
static void Log(string message, string level = "Info")
{
    Console.WriteLine($"[{level}] {message}");
}
```

Calls:

```csharp
Log("Started");
Log("Disk almost full", "Warning");
```

Use optional parameters when the behavior is the same and only a default varies.

---

## Overloads

Overloads are methods with the same name but different parameter lists.

```csharp
static string FormatUser(string first, string last)
{
    return $"{first} {last}";
}

static string FormatUser(string first, string last, string title)
{
    return $"{title} {first} {last}";
}
```

Use overloads when the method concept is the same but inputs differ.

---

## `params`

`params` allows a variable number of arguments.

```csharp
static decimal Sum(params decimal[] values)
{
    decimal total = 0m;

    foreach (decimal value in values)
    {
        total += value;
    }

    return total;
}
```

Use:

```csharp
decimal total = Sum(1m, 2m, 3m);
```

Do not overuse `params`; normal arrays/lists are often clearer for real data.

---

## Real Example: Order Line Methods

```csharp
static bool IsValidOrderLine(string sku, int quantity, decimal unitPrice)
{
    return !string.IsNullOrWhiteSpace(sku)
        && quantity > 0
        && unitPrice >= 0m;
}

static decimal CalculateLineTotal(int quantity, decimal unitPrice)
{
    return quantity * unitPrice;
}

static string FormatLine(string sku, int quantity, decimal unitPrice)
{
    decimal total = CalculateLineTotal(quantity, unitPrice);
    return $"{sku}: {quantity} x {unitPrice:C} = {total:C}";
}
```

Use:

```csharp
string sku = "KB-100";
int quantity = 2;
decimal unitPrice = 49.99m;

if (!IsValidOrderLine(sku, quantity, unitPrice))
{
    Console.WriteLine("Invalid order line.");
    return;
}

Console.WriteLine(FormatLine(sku, quantity, unitPrice));
```

---

## Common Mistakes

### Mistake 1: Too Many Parameters

If a method takes six related values, you may need a class or record soon.

### Mistake 2: `ref` For Convenience

Do not use `ref` just to avoid returning a value.

### Mistake 3: Method Does Too Much

A method that parses input, validates, calculates, saves, and prints should be
split.

---

## Chapter Checkpoint

You should now be able to answer:

- What is a method signature?
- What does a return type mean?
- What does `void` mean?
- When should you use `out`?
- Why should `ref` be rare?
- When are optional parameters useful?
- What are overloads?
- Why are small methods easier to test?

---

## Recap

- Methods name reusable behavior.
- Return values are usually clearer than mutating inputs.
- `out` fits Try-pattern methods.
- `ref` should be rare and intentional.
- Optional parameters provide simple defaults.
- Overloads support related behaviors with different inputs.

## Try It Yourself

Write:

- `IsValidOrderLine`
- `CalculateLineTotal`
- `TryReadPositiveQuantity`

Then use them from a console program.

---

[**Next ->** Conditionals, Switch, And Pattern Matching](./03-conditionals-switch-and-pattern-matching.md)  
[**<- Previous** Types, Variables, And Strings](./01-types-variables-and-strings.md)
