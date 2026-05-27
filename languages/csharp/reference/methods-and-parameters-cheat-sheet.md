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

## Examples

```csharp
static void Increment(ref int x) => x++;
static bool TryParse(string s, out int value) => int.TryParse(s, out value);
static int Sum(params int[] nums) => nums.Sum();
```

## Return guidance

- Return values for computation results.
- Use `out` only for try-pattern and multi-result scenarios.

---

[← C# Reference](./README.md)
