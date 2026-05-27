<h1 align="center">
    <img width="99" alt="C# logo" src="../../../../_assets/logos/cs.svg">
    <br>
    <b>C#</b>
</h1>

<!-- ===== HEAD NAV ===== -->
<div align="center">

[Home](../../../../README.md) · [C#](../../README.md) · [Chapter 02](./README.md)

</div>

---

# Methods, Parameters, and Returns

> Methods let you name logic once and reuse it safely.

**You will learn:**
- Method signatures and return types
- Parameter options: value, `ref`, `out`, and optional parameters
- When to return values versus changing parameters

**Before this page, you should know:** variables and basic types.

---

## Method signature

```csharp
static int Add(int a, int b)
{
    return a + b;
}
```

Signature includes visibility, return type, name, and parameters.

## Parameter alternatives

```csharp
static void Increment(ref int value) => value++;

static bool TryParseId(string input, out int id)
{
    return int.TryParse(input, out id);
}

static void Log(string message, string level = "Info")
{
    Console.WriteLine($"[{level}] {message}");
}

static bool IsInRange(in int value, int min, int max)
{
    return value >= min && value <= max;
}

static decimal Sum(params decimal[] values)
{
    decimal total = 0;
    foreach (var value in values)
        total += value;
    return total;
}
```

Use:
- value parameters by default
- `ref` only when caller variable must change
- `out` for try-pattern methods
- optional parameters for simple defaults
- `in` for readonly pass-by-reference semantics
- `params` for variable argument count APIs

## Return style guidelines

Prefer returning values instead of mutating input parameters when possible:

```csharp
static decimal ApplyDiscount(decimal price, decimal percent)
{
    return price - (price * percent);
}
```

Mutation-based APIs are harder to reason about, especially in larger teams.

## Overloads versus optional parameters

Both are valid, but use them intentionally:

- Overloads when behavior meaningfully differs.
- Optional parameters when behavior is the same and only defaults vary.

Example overload pair:

```csharp
static string FormatUser(string first, string last) => $"{first} {last}";
static string FormatUser(string first, string last, string title) => $"{title} {first} {last}";
```

## Parameter decision checklist

1. Start with value parameters.
2. Add return type for primary result.
3. Use `out` only for try-pattern or multi-result need.
4. Avoid `ref` unless required by API intent.
5. Keep signatures short and readable.

---

## Recap

- Keep signatures explicit and readable.
- Prefer return values over side effects when possible.
- Use `ref`/`out` sparingly and intentionally.

## Try it yourself

Write one method that returns a computed value and one `Try...` method using `out`.

---

<!-- ===== FOOT NAV ===== -->
<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Types, Variables, and Strings](./01-types-variables-and-strings.md) | [Chapter](./README.md) · [Track](../../README.md) · [Home](../../../../README.md) | [Conditionals, Switch, and Pattern Matching →](./03-conditionals-switch-and-pattern-matching.md) |

</div>
