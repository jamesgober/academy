<h1 align="center">
    <img width="99" alt="C logo" src="../../../../_assets/logos/c.svg">
    <br>
    <b>C</b>
</h1>

[Home](../../../../README.md) / [C](../../README.md) / [Chapter 05](./README.md)

---

# Quality Workflow And Release Checklist

> A C program is not done because it compiled once. C quality means the build is
> strict, behavior is checked, memory ownership is explained, and dangerous paths
> have been exercised.

**You will learn:**
- A repeatable local quality workflow
- What release gates matter for C
- How to document ownership
- How to check edge cases
- How to decide when a beginner C project is ready

**Before this page, you should know:** [Memory-Issue Triage Workflow](./04-memory-issue-triage-workflow.md)

---

## Local Quality Loop

For a one-file program:

```bash
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror -g main.c -o app
./app
```

For a multi-file program:

```bash
gcc -std=c17 -Wall -Wextra -Wpedantic -Werror -g \
    main.c sensor_log.c \
    -o sensor_log

./sensor_log
```

If sanitizer support exists:

```bash
gcc -std=c17 -g -O1 \
    -fsanitize=address,undefined \
    -fno-omit-frame-pointer \
    main.c sensor_log.c \
    -o sensor_log_san

./sensor_log_san
```

---

## Behavior Checks

Do not only test the happy path.

Check:

- empty input
- maximum capacity
- invalid length
- null pointer arguments when allowed
- failed allocation paths where possible
- invalid numeric values
- cleanup after early return

Example checklist for `SensorLog`:

```text
init with capacity 0 fails
add valid reading succeeds
add invalid battery fails
add past capacity fails
average empty log fails safely
free can be called once safely
```

---

## Ownership Note

Every C project with heap memory should be explainable.

Template:

```md
# Ownership

## Allocation

## Owner

## Borrowers

## Cleanup Function

## Error Paths

## Invalid After
```

Example:

```text
SensorLog owns items after sensor_log_init succeeds.
sensor_log_free releases items.
sensor_log_print borrows the log and does not free anything.
```

---

## Release Checklist

Before sign-off:

- strict compile passes
- program runs expected scenarios
- sanitizer run clean when available
- no known leaks
- no known out-of-bounds access
- no known use-after-free paths
- invalid inputs handled safely
- ownership note written
- edge cases documented
- build commands documented

---

## What "Clean" Means

Clean does not mean:

```text
I did not see a crash.
```

Clean means:

```text
I ran the agreed checks and they passed.
```

For C, those checks should include memory-focused checks whenever possible.

---

## Beginner Project Sign-Off Example

```md
# Release Check

## Commands

gcc -std=c17 -Wall -Wextra -Wpedantic -Werror -g main.c sensor_log.c -o sensor_log
./sensor_log

gcc -std=c17 -g -O1 -fsanitize=address,undefined -fno-omit-frame-pointer main.c sensor_log.c -o sensor_log_san
./sensor_log_san

## Result

Strict build passed.
Scenario run printed expected readings.
Sanitizer run produced no error report.

## Known Limits

The log has fixed capacity.
The app does not read from files yet.
```

---

## Common Mistakes

### Mistake 1: Only Building One File

If your project has `main.c` and `sensor_log.c`, build both.

### Mistake 2: No Edge Cases

If you never test full capacity, invalid input, or empty state, bugs hide there.

### Mistake 3: No Ownership Explanation

If you cannot explain who frees memory, the design is not clear enough yet.

---

## Chapter Checkpoint

You should now be able to answer:

- What commands belong in a local quality loop?
- What edge cases should C projects check?
- What belongs in an ownership note?
- Why is "no crash" not enough?
- What should a release checklist include?

---

## Recap

- C quality needs repeatable checks.
- Strict builds catch issues early.
- Sanitizers catch many runtime memory bugs.
- Edge cases matter.
- Ownership notes make memory design visible.
- Release readiness is stronger than "it compiled once."

## Try It Yourself

Write a release check for your capstone project with:

- exact commands
- expected output
- edge cases checked
- ownership summary
- known limits

---

[**Next ->** Chapter 05 Capstone](./06-chapter-05-capstone.md)  
[**<- Previous** Memory-Issue Triage Workflow](./04-memory-issue-triage-workflow.md)
