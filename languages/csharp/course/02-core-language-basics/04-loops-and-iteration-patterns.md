<h1 align="center">
    <img width="99" alt="C# logo" src="../../../../_assets/logos/cs.svg">
    <br>
    <b>C#</b>
</h1>

[Home](../../../../README.md) / [C#](../../README.md) / [Chapter 02](./README.md)

---

# Loops And Iteration Patterns

> Iteration is repetition with intent. Pick the loop that matches the shape of
> the work: count by index, process every item, or keep going until a condition
> changes.

**You will learn:**
- `for`
- `foreach`
- `while`
- `do while`
- `break`
- `continue`
- safe collection iteration
- common loop mistakes

**Before this page, you should know:** [Conditionals, Switch, And Pattern Matching](./03-conditionals-switch-and-pattern-matching.md)

---

## `foreach`

Use `foreach` when you want every item.

```csharp
var names = new List<string> { "Ada", "Grace", "Linus" };

foreach (string name in names)
{
    Console.WriteLine(name);
}
```

Read:

```text
For each name in names, print name.
```

This is the most readable loop for many collections.

---

## `for`

Use `for` when you need the index.

```csharp
for (int index = 0; index < names.Count; index++)
{
    Console.WriteLine($"{index}: {names[index]}");
}
```

Parts:

```text
int index = 0       start
index < names.Count keep going while true
index++             update
```

---

## `while`

Use `while` when the number of repetitions is unknown.

```csharp
while (true)
{
    Console.Write("Command: ");
    string? command = Console.ReadLine();

    if (command == "exit")
    {
        break;
    }

    Console.WriteLine($"You typed: {command}");
}
```

This is common in command loops.

---

## `do while`

`do while` runs at least once.

```csharp
string? input;

do
{
    Console.Write("Enter name: ");
    input = Console.ReadLine();
}
while (string.IsNullOrWhiteSpace(input));
```

Use sparingly. Many codebases use `while` and clear guard logic more often.

---

## `break` And `continue`

`break` exits the loop:

```csharp
foreach (string name in names)
{
    if (name == "Grace")
    {
        break;
    }
}
```

`continue` skips to the next iteration:

```csharp
foreach (int score in scores)
{
    if (score < 0)
    {
        continue;
    }

    Console.WriteLine(score);
}
```

Use both intentionally. Too many jumps can make loops hard to follow.

---

## Filtering Into A New List

```csharp
var rawScores = new List<int> { 100, -1, 80, 140, 60 };
var validScores = new List<int>();

foreach (int score in rawScores)
{
    if (score >= 0 && score <= 100)
    {
        validScores.Add(score);
    }
}
```

This pattern is important:

```text
create result
loop over source
keep valid items
use result
```

Later, LINQ can express the same idea:

```csharp
var validScores = rawScores
    .Where(score => score >= 0 && score <= 100)
    .ToList();
```

But loops are the best starting point.

---

## Accumulation

```csharp
decimal total = 0m;

foreach (decimal price in prices)
{
    total += price;
}
```

Average:

```csharp
if (prices.Count > 0)
{
    decimal average = total / prices.Count;
    Console.WriteLine(average);
}
```

Check for empty collections before dividing.

---

## Do Not Modify A List During `foreach`

This is risky and usually throws:

```csharp
foreach (string name in names)
{
    if (name.StartsWith("A"))
    {
        names.Remove(name);
    }
}
```

Better:

```csharp
names.RemoveAll(name => name.StartsWith("A"));
```

Or build a new list.

---

## Real Example: Command Loop

```csharp
var items = new List<string>();

while (true)
{
    Console.Write("Command (add/list/exit): ");
    string? command = Console.ReadLine();

    if (command == "exit")
    {
        break;
    }

    if (command == "add")
    {
        Console.Write("Item: ");
        string item = Console.ReadLine() ?? "";

        if (!string.IsNullOrWhiteSpace(item))
        {
            items.Add(item);
        }

        continue;
    }

    if (command == "list")
    {
        foreach (string item in items)
        {
            Console.WriteLine($"- {item}");
        }

        continue;
    }

    Console.WriteLine("Unknown command.");
}
```

This is the shape of many beginner console apps.

---

## Common Mistakes

### Mistake 1: Off-By-One

Wrong:

```csharp
for (int index = 0; index <= names.Count; index++)
{
    Console.WriteLine(names[index]);
}
```

Use `<`, not `<=`.

### Mistake 2: Infinite Loop

```csharp
int count = 0;

while (count < 10)
{
    Console.WriteLine(count);
}
```

`count` never changes.

### Mistake 3: Changing Collection During `foreach`

Use `RemoveAll`, a `for` loop with care, or build a new list.

---

## Chapter Checkpoint

You should now be able to answer:

- When is `foreach` best?
- When is `for` best?
- When is `while` best?
- What does `break` do?
- What does `continue` do?
- What is an off-by-one error?
- Why should you avoid modifying a list during `foreach`?

---

## Recap

- `foreach` is best for processing every item.
- `for` is best when you need indexes.
- `while` is best when repetition depends on a condition.
- `break` exits a loop.
- `continue` skips one iteration.
- Loops can filter, accumulate, search, and drive command prompts.

## Try It Yourself

Build a small command loop:

- `add` stores a task title
- `list` prints all task titles
- `exit` stops
- blank titles are ignored

---

[**Next ->** Chapter 02 Checkpoint](./05-chapter-02-checkpoint.md)  
[**<- Previous** Conditionals, Switch, And Pattern Matching](./03-conditionals-switch-and-pattern-matching.md)
