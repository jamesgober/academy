# Debugging and Sanitizer Quick Guide

Quick workflow for memory-issue investigation in C.

## Triage flow

1. Reproduce issue.
2. Capture warning/sanitizer output.
3. Classify bug type.
4. Fix ownership or bounds logic.
5. Rebuild and rerun tools.

## Build with AddressSanitizer

```bash
gcc -g -O1 -fsanitize=address -fno-omit-frame-pointer main.c -o app
./app
```

## Common report interpretations

- use-after-free: pointer used after cleanup
- heap-buffer-overflow: write/read past allocated region
- double-free: memory released more than once
- leak report: allocation not freed on at least one path

## Fix discipline

- patch root cause, not only symptom
- retest full workflow after each fix
- keep minimal reproducer for future regressions

---

[← Reference Index](./README.md) · [C](../README.md)
