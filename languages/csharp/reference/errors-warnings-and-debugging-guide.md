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
- `CS1503`: argument type mismatch
- `CS7036`: required argument missing
- `CS0246`: type/namespace not found

## Warning policy

Warnings are not cosmetic. Nullability warnings often predict runtime failures.

Treat warnings as backlog items at minimum, and as blockers for critical paths.

## First-pass compiler triage

1. Fix topmost syntax or missing-symbol error.
2. Rebuild.
3. Repeat until errors are zero.
4. Triage warnings by category (nullability, style, obsolete APIs).

## Debug workflow

1. Reproduce reliably.
2. Fix first error first.
3. Use breakpoints and variable inspection.
4. Add logs with context IDs.
5. Add tests after fix.

## Repro checklist for bug reports

- exact input values
- environment details (OS, SDK version)
- expected behavior versus actual behavior
- first failing stack trace section

---

[← C# Reference](./README.md)
