<h1 align="center">
    <img width="99" alt="C logo" src="../../../../_assets/logos/c.svg">
    <br>
    <b>C</b>
</h1>

[Home](../../../../README.md) / [C](../../README.md) / [Chapter 05](./README.md)

---

# Quality Workflow and Release Checklist

> A C project is not release-ready just because it compiles once.

**You will learn:**
- A practical quality command loop
- Release gates for C projects
- Memory-safety gates that should be non-negotiable

**Before this page, you should know:** [Memory-Issue Triage Workflow](./04-memory-issue-triage-workflow.md)

---

## Suggested local workflow

1. strict compile with warnings enabled
2. run tests or scenario checks
3. run sanitizer build and execute
4. review logs and clean remaining issues

## Release checklist

- strict compile clean
- sanitizer run clean
- no known leaks or invalid-memory findings
- edge-case behavior documented
- changelog/versioning updated

> [!IMPORTANT]
> In C, memory-safety checks are core quality checks, not optional extras.

---

## Recap

- Repeatable workflows prevent quality drift.
- Release readiness includes memory safety.
- "Compiles" and "safe enough to ship" are different states.

## Try it yourself

Make a one-page checklist for your own C project and run it once end to end.

---

[**Next ->** Chapter 05 Capstone](./06-chapter-05-capstone.md)
[**<- Previous** Memory-Issue Triage Workflow](./04-memory-issue-triage-workflow.md)


