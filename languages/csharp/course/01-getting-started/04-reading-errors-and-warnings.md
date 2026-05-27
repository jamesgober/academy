<h1 align="center">
    <img width="99" alt="C# logo" src="../../../../_assets/logos/cs.svg">
    <br>
    <b>C#</b>
</h1>

<!-- ===== HEAD NAV ===== -->
<div align="center">

[Home](../../../../README.md) · [C#](../../README.md) · [Chapter 01](./README.md)

</div>

---

# Reading Errors and Warnings

> Stop guessing: learn to decode compiler output and fix issues systematically.

**You will learn:**
- How to read C# compiler diagnostics
- How warnings differ from errors
- A practical triage workflow

**Before this page, you should know:** `dotnet build` basics.

---

## Diagnostic format

Typical output:

```text
Program.cs(12,17): error CS0103: The name 'totla' does not exist in the current context
```

Read in this order:
1. File and location `(12,17)`.
2. Severity (`error` vs `warning`).
3. Diagnostic ID (`CS0103`).
4. Message details.

## Warnings matter

Example warning:

```text
warning CS8602: Dereference of a possibly null reference.
```

Warnings often indicate future runtime bugs. Treat them as real work.

## Triage loop

1. Fix the first error first.
2. Rebuild.
3. Repeat until clean.
4. Resolve warnings.

> [!IMPORTANT]
> One syntax mistake can create many downstream errors. Fixing topmost errors often clears most of the list.

---

## Recap

- Read file, line, ID, then message.
- Fix first error before touching later ones.
- Do not ignore nullability warnings.

## Try it yourself

Introduce a typo in a variable name, build, then resolve the resulting `CS0103`.

---

<!-- ===== FOOT NAV ===== -->
<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Project Commands: new, build, run](./03-project-commands-new-build-run.md) | [Chapter](./README.md) · [Track](../../README.md) · [Home](../../../../README.md) | [Chapter 01 Checkpoint →](./05-chapter-01-checkpoint.md) |

</div>
