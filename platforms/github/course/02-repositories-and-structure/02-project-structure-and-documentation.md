<h1 align="center">
    <img width="99" alt="GitHub logo" src="../../../../_assets/logos/github.svg">
    <br>
    <b>GitHub</b>
</h1>

<div align="center">

[Home](../../../../README.md) · [GitHub](../../README.md) · [Chapter 02](./README.md)

</div>

---

# Project Structure and Documentation

> Good structure helps humans and tools find what they need quickly.

**You will learn:**
- A practical repository layout for beginners
- Which docs files are mandatory vs optional
- How docs reduce repetitive support work

**Before this page, you should know:** [Repository Naming and Scope](./01-repository-naming-and-scope.md)

---

## Recommended starter layout

```text
repo/
├── src/                    project code
├── docs/                   long-form documentation
├── .github/                templates and workflows
├── README.md               entry point for users
├── CONTRIBUTING.md         contributor process
├── CHANGELOG.md            notable changes over time
└── LICENSE                 usage permissions
```

## Mandatory docs for public repos

- `README.md`
- `LICENSE`
- `CHANGELOG.md`

Strongly recommended:
- `CONTRIBUTING.md`
- `CODE_OF_CONDUCT.md`

> [!TIP]
> If users ask the same question twice, that answer belongs in docs.

## Use docs folders intentionally

- Keep quick-start steps in `README.md`.
- Keep detailed guides in `docs/`.
- Link from README to docs so navigation is obvious.

---

## Recap

- Predictable layout lowers onboarding time.
- Core docs files are part of project quality, not optional extras.
- README should link to deeper docs.

## Try it yourself

Create a `docs/` folder and move one long explanation from README into a linked
page under `docs/`.

---

<div align="center">

| Previous | Up | Next |
|:---------|:--:|-----:|
| [← Repository Naming and Scope](./01-repository-naming-and-scope.md) | [Chapter 02](./README.md) · [GitHub](../../README.md) · [Home](../../../../README.md) | [Licenses for Beginners →](./03-licenses-for-beginners.md) |

</div>
