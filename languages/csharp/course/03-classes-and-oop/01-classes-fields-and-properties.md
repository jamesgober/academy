<h1 align="center">
    <img width="99" alt="C# logo" src="../../../../_assets/logos/cs.svg">
    <br>
    <b>C#</b>
</h1>

[Home](../../../../README.md) / [C#](../../README.md) / [Chapter 03](./README.md)

---

# Classes, Fields, And Properties

> A C# class should make invalid states hard to create. Fields store the internal
> state. Properties expose controlled access. Methods describe useful behavior.

**You will learn:**
- What classes are for
- The difference between fields and properties
- Why fields are usually private
- How auto-properties work
- How to validate property changes
- How to design beginner-friendly objects with clear rules

**Before this page, you should know:** [Methods, Parameters, And Returns](../02-core-language-basics/02-methods-parameters-and-returns.md)

---

## The Beginner Mental Model

A class bundles:

```text
data + behavior + rules
```

Example:

```text
Product
  data:
    SKU
    name
    price

  behavior:
    change price
    print label

  rules:
    SKU cannot be blank
    name cannot be blank
    price cannot be negative
```

The rules are the reason classes matter. Without rules, any part of the program
can put an object into nonsense state.

---

## Fields

A field is a variable stored inside an object.

```csharp
public class Product
{
    private string _name = "";
    private decimal _price;
}
```

This course uses `_camelCase` for private fields:

```text
_price is object state.
price is probably a parameter or local variable.
```

Beginner rule:

```text
Keep fields private unless you have a strong reason not to.
```

---

## Properties

A property is the public access point for state.

```csharp
public class Product
{
    public string Name { get; set; } = "";
}
```

This is an auto-property. C# creates the hidden backing storage for you.

Use auto-properties when simple get/set behavior is enough.

---

## Read-Only Public Property

```csharp
public class Product
{
    public string Sku { get; }

    public Product(string sku)
    {
        Sku = sku;
    }
}
```

`Sku` can be set by the constructor, but outside code cannot change it later.

This is useful for identity-like values.

---

## Validated Property

```csharp
public class Product
{
    private decimal _price;

    public decimal Price
    {
        get => _price;
        set
        {
            if (value < 0)
            {
                throw new ArgumentOutOfRangeException(
                    nameof(value),
                    "Price cannot be negative."
                );
            }

            _price = value;
        }
    }
}
```

`value` is the incoming value assigned to the property:

```csharp
product.Price = 9.99m;
```

Inside the setter:

```text
value is 9.99m
```

---

## Complete Product Example

```csharp
public class Product
{
    private decimal _price;

    public Product(string sku, string name, decimal price)
    {
        if (string.IsNullOrWhiteSpace(sku))
        {
            throw new ArgumentException("SKU is required.", nameof(sku));
        }

        if (string.IsNullOrWhiteSpace(name))
        {
            throw new ArgumentException("Name is required.", nameof(name));
        }

        Sku = sku;
        Name = name;
        Price = price;
    }

    public string Sku { get; }

    public string Name { get; private set; }

    public decimal Price
    {
        get => _price;
        private set
        {
            if (value < 0)
            {
                throw new ArgumentOutOfRangeException(
                    nameof(value),
                    "Price cannot be negative."
                );
            }

            _price = value;
        }
    }

    public void Rename(string name)
    {
        if (string.IsNullOrWhiteSpace(name))
        {
            throw new ArgumentException("Name is required.", nameof(name));
        }

        Name = name;
    }

    public void ChangePrice(decimal price)
    {
        Price = price;
    }

    public string Label()
    {
        return $"{Sku}: {Name} ({Price:C})";
    }
}
```

Use:

```csharp
var keyboard = new Product("KB-100", "Keyboard", 49.99m);

keyboard.ChangePrice(44.99m);
keyboard.Rename("Mechanical Keyboard");

Console.WriteLine(keyboard.Label());
```

---

## Why `private set`?

```csharp
public string Name { get; private set; }
```

Outside code can read:

```csharp
Console.WriteLine(product.Name);
```

But outside code cannot directly assign:

```csharp
product.Name = ""; // not allowed outside the class
```

Instead, callers must use:

```csharp
product.Rename("New Name");
```

That lets the class validate the change.

---

## Properties Versus Methods

Use a property when the operation feels like reading or setting data:

```csharp
product.Name
product.Price
order.Total
```

Use a method when the operation is behavior:

```csharp
product.Rename("Keyboard")
order.AddItem(item)
cart.Checkout()
```

Avoid surprising expensive work inside properties. A property should usually be
cheap and predictable.

---

## Object Initializers

Some types are designed for object initializer syntax:

```csharp
public class UserProfile
{
    public string DisplayName { get; set; } = "";
    public string Email { get; set; } = "";
}
```

Use:

```csharp
var profile = new UserProfile
{
    DisplayName = "Ada",
    Email = "ada@example.com"
};
```

This is convenient, but it may allow invalid temporary states. For important
rules, prefer constructors and controlled methods.

---

## Common Mistakes

### Mistake 1: Public Fields

```csharp
public class Product
{
    public decimal Price;
}
```

Any code can set:

```csharp
product.Price = -100;
```

Use a property or method with validation.

### Mistake 2: Setters That Bypass Rules

```csharp
public decimal Price { get; set; }
```

This is fine only if any price value is valid. If negative prices are invalid,
validate.

### Mistake 3: Giant Classes

If a class manages users, orders, database access, email, logging, and reports,
it has too many jobs.

Small classes are easier to test and easier to understand.

---

## Mini Project: User Profile

Create a `UserProfile` class.

Requirements:

- `Username` cannot be blank
- `Email` cannot be blank
- outside code can read both
- outside code must use `ChangeEmail` to update email
- `DisplayLabel()` returns `"username <email>"`

Starter:

```csharp
public class UserProfile
{
    public UserProfile(string username, string email)
    {
        if (string.IsNullOrWhiteSpace(username))
        {
            throw new ArgumentException("Username is required.", nameof(username));
        }

        Username = username;
        ChangeEmail(email);
    }

    public string Username { get; }
    public string Email { get; private set; } = "";

    public void ChangeEmail(string email)
    {
        if (string.IsNullOrWhiteSpace(email))
        {
            throw new ArgumentException("Email is required.", nameof(email));
        }

        Email = email;
    }

    public string DisplayLabel()
    {
        return $"{Username} <{Email}>";
    }
}
```

---

## Chapter Checkpoint

You should now be able to answer:

- What is a field?
- What is a property?
- Why are fields usually private?
- What does `private set` do?
- When should validation happen?
- When should behavior be a method instead of a property?
- Why can object initializers be risky for rule-heavy objects?

---

## Recap

- Classes bundle data, behavior, and rules.
- Fields store internal state.
- Properties expose controlled access.
- Constructors should create valid objects.
- `private set` allows public reading but controlled mutation.
- Methods are better for behavior and rule-based changes.

## Try It Yourself

Build a `BankAccount` class:

- `Owner` is required
- `Balance` can be read publicly
- `Deposit(decimal amount)` rejects non-positive amounts
- `Withdraw(decimal amount)` returns `false` if funds are insufficient
- outside code cannot directly set `Balance`

---

[**Next ->** Constructors And Object Lifecycle](./02-constructors-and-object-lifecycle.md)  
[**<- Previous** Chapter Start](./README.md)
