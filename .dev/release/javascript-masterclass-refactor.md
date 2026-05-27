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

## Validation

Validation run:

```powershell
BROKEN_LINK_COUNT=0
ODD_FENCE_COUNT=0
MERMAID_BLOCK_COUNT=0
```
