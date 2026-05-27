# 2026-05-27 JavaScript and Node Separation Fix

## Summary
Corrected JavaScript track drift where Node.js-specific content leaked into JavaScript-only lessons.

## Changes
- Reworked JavaScript Chapter 01 to browser-first environment setup and execution guidance.
- Removed Node.js and npm-specific commands and language from JavaScript lesson content.
- Reworked Chapter 05 testing/debugging content to be framework-agnostic and JavaScript-only.
- Updated JavaScript reference tooling page to avoid Node/npm-specific command assumptions.
- Renamed lesson files to remove Node-specific naming:
  - `01-installing-nodejs-and-running-javascript.md` -> `01-setting-up-a-javascript-practice-environment.md`
  - `01-testing-fundamentals-with-node-test-runner.md` -> `01-testing-fundamentals.md`
- Updated all affected chapter and footer links to renamed files.

## Validation
- Link integrity scan returned `BROKEN_LINK_COUNT=0` for `languages/javascript`.
- Diagnostics check returned no errors for `languages/javascript`.
- Regex scan found no `Node/node/npm/package.json/--test/--inspect` tokens in JavaScript track markdown content.
