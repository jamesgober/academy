# Errors, Warnings, and Debugging Guide

## Diagnostic format

```text
Program.cs(10,13): error CS0103: The name 'x' does not exist in the current context
```

Read:
1. file and location
2. severity
3. diagnostic ID
4. message

## Common IDs

- `CS0103`: unknown name/symbol
- `CS1002`: syntax issue, usually missing `;`
- `CS8618`: non-nullable member not initialized
- `CS8602`: possible null dereference

## Warning policy

Warnings are not cosmetic. Nullability warnings often predict runtime failures.

## Debug workflow

1. Reproduce reliably.
2. Fix first error first.
3. Use breakpoints and variable inspection.
4. Add logs with context IDs.
5. Add tests after fix.

---

[← C# Reference](./README.md)
