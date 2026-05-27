# Memory Safety and Leak Prevention Checklist

Use this checklist during code review and before release.

## Allocation and cleanup

- Every `malloc`/`calloc` has exactly one matching `free`.
- Error paths still run cleanup.
- Cleanup order is explicit and documented.

## Pointer validity

- No dereference of uninitialized pointers.
- No use-after-free.
- Freed pointers reset to `NULL` where practical.

## Bounds and strings

- Array indexes validated.
- Buffer capacity tracked with data.
- String writes are bounded and terminator-aware.

## Bug-class prompts

- leak: where was allocation not freed?
- dangling: where does pointer outlive target?
- double free: where can cleanup run twice?
- out-of-bounds: where is index/length unchecked?

## Final gate

- Strict warning build clean.
- Sanitizer run clean.
- No unresolved memory warnings.

---

[← Reference Index](./README.md) · [C](../README.md)
