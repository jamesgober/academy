# JavaScript Masterclass Refactor

## Summary

Started the JavaScript track refactor as a browser-first, beginner-friendly
masterclass. The focus is frontend JavaScript: arrays and objects for state,
DOM creation and manipulation, events, rendering, object/class/prototype
patterns, virtual DOM concepts, and practical reference lookup.

## Concrete Changes

- Reframed the JavaScript track README around browser/web JavaScript.
- Expanded arrays and objects into a full lesson covering mutating versus
  non-mutating APIs, method parameters, examples, object updates, destructuring,
  nested state updates, and real UI state transformations.
- Expanded objects/classes/prototypes into a deeper lesson covering object
  categories, prototype lookup, classes, private fields, static methods,
  inheritance, factories, composition, and DOM objects.
- Rebuilt the DOM/API lesson around real browser work: querying, creating,
  updating, appending, removing, event listeners, event delegation, state-driven
  rendering, fetch-to-UI flow, and a tiny virtual DOM element creator.
- Rebuilt core JavaScript references for arrays/objects, DOM/events,
  objects/classes/prototypes, virtual DOM, and the reference index.
- Replaced JavaScript Mermaid visual models with plain text diagrams so visual
  models render in basic Markdown viewers.
- Updated JavaScript lesson footers to use the Rust-style next-first navigation.
- Expanded setup/tooling with DevTools, local server, script tags versus module
  scripts, project structure, and a light Node/npm context.
- Expanded variables/types/coercion with `null`, `undefined`, `symbol`,
  `bigint`, truthiness, strict equality, parsing, and boundary validation.
- Expanded functions and closures with callbacks, higher-order functions, pure
  functions, side effects, arrow `this`, UI event closures, module scope, and
  private state.
- Expanded modules with browser module loading, named/default exports, import
  paths, barrels, circular imports, and common import errors.
- Expanded async with event loop ordering, microtasks/tasks, Promise lifecycle,
  `Promise.all`, `Promise.allSettled`, cancellation, retries, UI async states,
  Web Workers, and Node context.
- Expanded fetch/browser API coverage with headers, JSON bodies, query params,
  `FormData`, `localStorage`, `sessionStorage`, JSON parsing failure cases, and
  storage safety notes.
- Expanded testing with pure function tests, DOM tests, mocks/fakes, user
  interaction tests, fake fetches, and browser app testing checklist.
- Replaced the capstone checklist with a full guided Task Tracker Web App build.
- Added a Chapter 02 checkpoint and strengthened final checkpoint review
  criteria with hints and solution direction.

## Validation

Validation run:

```powershell
BROKEN_LINK_COUNT=0
ODD_FENCE_COUNT=0
MERMAID_BLOCK_COUNT=0

Lessons        : 26
CourseWords    : 14798
AvgLessonWords : 569
ReferencePages : 11
ReferenceWords : 4078
AvgRefWords    : 371
```
