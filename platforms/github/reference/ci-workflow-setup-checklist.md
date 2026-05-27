<h1 align="center">
    <img width="99" alt="GitHub logo" src="../../../_assets/logos/github.svg">
    <br>
    <b>GitHub</b>
</h1>

<div align="center">

[Home](../../../README.md) · [Track](../README.md) · [Reference Index](./README.md)

</div>

---

# CI Workflow Setup Checklist

> Fast checklist for creating maintainable, beginner-safe CI.

## Workflow basics

- CI file exists at `.github/workflows/ci.yml`.
- Workflow triggers on pull requests.
- Workflow also runs on pushes to main.

## Core checks

- Build/check command runs.
- Test command runs.
- Lint command runs.
- Format check runs.

## Reliability

- Dependencies are cached where appropriate.
- Workflow uses pinned major versions for actions.
- Workflow runtime is reasonable for contributor experience.

## Branch protection

- Main branch requires CI status checks.
- Direct pushes to main are restricted in team repos.

## Troubleshooting first steps

- Re-run failed job once to rule out transient runner/network failures.
- Inspect failing step logs before changing code.
- Keep failing output copied into PR discussion when asking for help.

---

<div align="center">

[← Reference Index](./README.md) · [Track](../README.md) · [Home](../../../README.md)

</div>
