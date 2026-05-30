# Methods and Parameters Cheat Sheet

## Signature shape

```csharp
public static int Add(int a, int b)
```

## Parameter kinds

- Value parameter: default and preferred
- `ref`: caller variable may be modified
- `out`: caller receives assigned value
- `in`: pass by reference as readonly
- `params`: variable number of arguments
- optional/default parameter: caller may omit trailing argument
- named argument: caller specifies parameter names for clarity

## Examples

```csharp
static void Increment(ref int x) => x++;
static bool TryParse(string s, out int value) => int.TryParse(s, out value);
static int Sum(params int[] nums) => nums.Sum();
static string Label(string id, string prefix = "ORD") => $"{prefix}-{id}";
static (int Count, decimal Total) Snapshot(List<decimal> values) => (values.Count, values.Sum());
```

Named argument example:

```csharp
var label = Label(id: "1001", prefix: "INV");
```

## Return guidance

- Return values for computation results.
- Use `out` only for try-pattern and multi-result scenarios.

Tuple returns are often cleaner than multiple `out` parameters when meaning is clear.

## Common anti-patterns

- Too many parameters in one method signature
- Using `ref` for performance without measuring
- Using `out` where a return object/tuple is clearer
- Optional parameters that hide important behavior differences

---

[C# Reference](./README.md)

