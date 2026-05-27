# Language Track Quality Gates

Use this as a strict pass/fail checklist for every language track (new or refactor).
A track is not complete unless all gates pass.

## Scope

Applies to every language under `languages/<lang>/`.

## Gate 1: Required structure exists

Required directories:
- `languages/<lang>/course/`
- `languages/<lang>/reference/`

Required chapter structure (minimum):
- 5 chapter folders under `course/`
- each chapter has `README.md`
- each chapter has ordered lesson pages with working links

Pass criteria:
- track README links to all chapters and reference index
- chapter README files link to all lessons in order

## Gate 2: Tutorial page chrome consistency

Every lesson page in `course/**` must include:
- logo header block
- head nav block
- footer nav block
- previous/up/next navigation table

Pass criteria:
- no missing head/foot nav blocks
- no malformed navigation tables
- no duplicate footer blocks

## Gate 3: Mandatory pedagogy coverage

Each track must include explicit pages covering:
- installation/toolchain acquisition with official links
- command workflow for build/run/test
- compiler/runtime errors and warnings interpretation
- condition variants (if-only, else-if, nested, ternary/switch where available)
- methods/functions with parameter alternatives and return strategy
- capstone with expected outcomes and checklist

Pass criteria:
- all required topics exist in course pages
- lessons include examples and practical workflows

## Gate 4: Reference quality baseline

Reference section must include practical lookup pages for:
- commands and build flags
- types and strings
- methods/functions and parameters
- conditionals and loops
- debugging/errors/warnings triage

Pass criteria:
- each reference page has runnable or realistic snippets
- references provide usage guidance, not just term lists

## Gate 5: Visual model integrity

If a page contains a visual model section, Mermaid blocks must not be empty.

Pass criteria:
- no empty Mermaid blocks
- diagrams reflect actual concept flow

## Gate 6: Validation and link integrity

Required checks before completion:
- markdown diagnostics clean (`get_errors`)
- no broken local markdown links
- chapter and lesson navigation resolves correctly

Pass criteria:
- zero diagnostics errors
- zero broken local links

## Gate 7: NOTES alignment

Content must align with `.dev/NOTES.md` goals:
- beginner-friendly and professional
- explain "why" not just "what"
- include alternatives, pitfalls, and troubleshooting patterns
- avoid shallow overview-only pages for core concepts

Pass criteria:
- lessons are detailed enough for novice-to-engineer progression
- hard areas include explicit triage guidance and examples

## Gate 8: Release note requirement

Every substantial update must add a release note in `.dev/release/`.

Pass criteria:
- release note includes summary, concrete changes, and validation output
- no public docs include `.dev/` internal path references

## Suggested completion command checklist

Use equivalent checks for each track before sign-off:

```powershell
# 1) markdown diagnostics
# get_errors on languages/<lang>

# 2) broken link scan (local targets)
$files = Get-ChildItem -Path languages/<lang> -Recurse -Filter *.md
$missing = 0
foreach ($f in $files) {
  $dir = Split-Path $f.FullName -Parent
  $raw = Get-Content -Raw $f.FullName
  $matches = [regex]::Matches($raw, '\[[^\]]+\]\(([^\)]+)\)')
  foreach ($m in $matches) {
    $target = $m.Groups[1].Value
    if ($target -match '^https?://|^#|^mailto:') { continue }
    $resolved = Join-Path $dir $target
    if (-not (Test-Path $resolved)) {
      $missing++
      Write-Output "BROKEN_LINK: $($f.FullName) -> $target"
    }
  }
}
Write-Output "BROKEN_LINK_COUNT=$missing"
```
