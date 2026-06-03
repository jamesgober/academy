<h1 align="center">
    <img width="99" alt="C logo" src="../../../../_assets/logos/c.svg">
    <br>
    <b>C</b>
</h1>

[Home](../../../../README.md) / [C](../../README.md) / [Chapter 05](./README.md)

---

# Memory-Issue Triage Workflow

> C memory bugs become less scary when you classify them, trace ownership, and
> verify the fix with the same tools that exposed the problem.

**You will learn:**
- How to classify memory bugs
- How to trace allocation ownership
- How to find lifetime mismatches
- How to fix cleanup paths
- How to verify the fix
- How to write a short triage note

**Before this page, you should know:** [Address Sanitizer And Leak Detection](./03-address-sanitizer-and-leak-detection.md)

---

## Triage Order

1. Reproduce the issue.
2. Save the exact compiler/sanitizer/crash output.
3. Classify the bug.
4. Find the allocation or object lifetime.
5. Find the invalid access or missing cleanup.
6. Fix the ownership design.
7. Rerun strict build.
8. Rerun sanitizer or scenario.
9. Add a test or note so the bug stays fixed.

---

## Classification Table

| Bug | Symptom | Core question |
|---|---|---|
| leak | allocation not freed | where should cleanup happen? |
| use-after-free | read/write after `free` | who kept a stale pointer? |
| double free | same allocation freed twice | who thinks they own it? |
| out-of-bounds | invalid index/pointer arithmetic | what is the valid length? |
| uninitialized read | value used before set | where should initialization happen? |
| null dereference | pointer is `NULL` | who failed to check allocation/input? |

---

## Ownership Trace

For every heap pointer, write:

```text
allocated by:
owned by:
borrowed by:
freed by:
invalid after:
```

Example:

```text
allocated by: sensor_log_init
owned by: struct SensorLog
borrowed by: sensor_log_print, sensor_log_average_temperature
freed by: sensor_log_free
invalid after: sensor_log_free returns
```

This makes memory bugs easier to reason about.

---

## Fix Leaks

Leak shape:

```c
int *values = malloc(10 * sizeof(*values));
if (values == NULL) {
    return 1;
}

if (something_failed()) {
    return 1;
}

free(values);
```

Fix:

```c
int *values = malloc(10 * sizeof(*values));
if (values == NULL) {
    return 1;
}

if (something_failed()) {
    free(values);
    values = NULL;
    return 1;
}

free(values);
values = NULL;
```

Every exit path after successful allocation must clean up.

---

## Fix Use-After-Free

Bug:

```c
free(values);
printf("%d\n", values[0]);
```

Fix options:

- use the value before freeing
- move `free` later
- set pointer to `NULL` after free
- remove stale borrowed pointer
- redesign ownership so access cannot outlive the owner

Setting to `NULL` helps catch accidental reuse, but it does not fix all aliases.

If another pointer still points to the freed allocation, that alias is still
dangerous.

---

## Fix Double Free

Bug:

```c
free(values);
free(values);
```

Fix:

```c
free(values);
values = NULL;
```

But also ask:

```text
Why did cleanup run twice?
Are there two owners?
Should one function borrow instead of own?
```

---

## Fix Out-Of-Bounds

Bug:

```c
for (size_t i = 0; i <= count; i++) {
    printf("%d\n", values[i]);
}
```

Fix:

```c
for (size_t i = 0; i < count; i++) {
    printf("%d\n", values[i]);
}
```

Ask:

- what is the valid range?
- where did `count` come from?
- is `count` trusted?
- does the array really have `count` elements?

---

## Triage Note Template

```md
# Memory Bug Triage

## Symptom

## Tool Output

## Classification

## Allocation / Owner

## Invalid Access Or Missing Cleanup

## Fix

## Verification
```

Writing the note forces clear thinking.

---

## Verification

After a fix:

```bash
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror -g main.c -o app
./app
```

Then sanitizer when available:

```bash
gcc -std=c17 -g -O1 -fsanitize=address,undefined -fno-omit-frame-pointer main.c -o app_san
./app_san
```

Do not call a memory bug fixed until the original reproduction is clean.

---

## Chapter Checkpoint

You should now be able to answer:

- What is the first step in triage?
- How do you classify a memory bug?
- What questions belong in an ownership trace?
- Why is setting a pointer to `NULL` not a complete fix for aliases?
- How do you verify a memory bug fix?
- What belongs in a triage note?

---

## Recap

- Classify the bug before editing.
- Trace ownership and borrowing.
- Fix cleanup paths, not just crash lines.
- Use strict builds and sanitizers for verification.
- A short triage note makes debugging less chaotic.

## Try It Yourself

Pick one bug type and write a triage note:

```text
symptom -> classification -> owner -> fix -> verification
```

---

[**Next ->** Quality Workflow And Release Checklist](./05-quality-workflow-and-release-checklist.md)  
[**<- Previous** Address Sanitizer And Leak Detection](./03-address-sanitizer-and-leak-detection.md)
