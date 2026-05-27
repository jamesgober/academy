<h1 align="center">
    <img width="99" alt="GitHub logo" src="../../../_assets/logos/github.svg">
    <br>
    <b>GitHub</b>
</h1>

<div align="center">

[Home](../../../README.md) · [Track](../README.md) · [Reference Index](./README.md)

</div>

---

# Semantic Versioning and Changelog Guide

> Quick lookup for MAJOR/MINOR/PATCH and practical changelog format.

## Versioning quick map

| Segment | Meaning | Example |
|---------|---------|---------|
| MAJOR | breaking changes | `2.0.0` |
| MINOR | backward-compatible features | `1.4.0` |
| PATCH | backward-compatible fixes | `1.4.3` |

## Pre-release labels

```text
x.y.z-alpha.0
x.y.z-beta.0
x.y.z-rc.0
```

Recommended style:

```text
1.0.0-beta.1
```

## Suggested 0.x development policy

- `0.1.0`: scaffolding and setup baseline.
- `0.2.0` to `0.8.0`: milestone-driven development.
- `0.8.x`: alpha/local testing runway.
- `0.9.x`: beta and release candidate stage.

## Changelog starter format

```markdown
## [0.3.2] - 2026-05-27
### Added
- Added contributor onboarding guide.

### Changed
- Updated branch strategy section.

### Fixed
- Corrected release-link formatting.
```

Spec: <https://semver.org>

---

<div align="center">

[← Reference Index](./README.md) · [Track](../README.md) · [Home](../../../README.md)

</div>
