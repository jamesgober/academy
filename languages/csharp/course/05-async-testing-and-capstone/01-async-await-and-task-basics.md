<h1 align="center">
    <img width="99" alt="C# logo" src="../../../../_assets/logos/cs.svg">
    <br>
    <b>C#</b>
</h1>

<!-- ===== HEAD NAV ===== -->
<div align="center">

[Home](../../../../README.md) · [C#](../../README.md) · [Chapter 05](./README.md)

</div>

---

# Async/Await and Task Basics

> Async code keeps apps responsive while waiting on IO.

**You will learn:**
- How `Task` and `async/await` work together
- Why blocking calls hurt scalability
- Common async mistakes to avoid

**Before this page, you should know:** methods and exceptions.

---

## Async example

```csharp
public async Task<string> LoadTextAsync(string path)
{
    string content = await File.ReadAllTextAsync(path);
    return content;
}
```

Guidance:
- use `await` instead of `.Result` and `.Wait()`
- propagate async outward where possible
- name async methods with `Async` suffix

## Common mistake

Blocking async result:

```csharp
var text = LoadTextAsync("data.txt").Result; // avoid in app code
```

Can cause deadlocks in some contexts and reduces responsiveness.

---

## Recap

- Async is for waiting efficiently, not for faster CPU math.
- Use `await` instead of blocking patterns.
- Keep naming and flow consistent for maintainability.

## Try it yourself

Convert one synchronous file operation to async and measure code readability difference.

---

<!-- ===== FOOT NAV ===== -->
<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Chapter Start](./README.md) | [Chapter](./README.md) · [Track](../../README.md) · [Home](../../../../README.md) | [Unit Testing with xUnit →](./02-unit-testing-with-xunit.md) |

</div>
