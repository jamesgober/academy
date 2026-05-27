# JavaScript Reference

[Home](../../../README.md) / [JavaScript](../README.md) / Reference

---

> Lookup for browser-first JavaScript: DOM, events, arrays, objects, classes, prototypes, async API work, errors, and tooling.

Use the course when learning a concept for the first time. Use this reference
when you need method names, parameters, examples, and risk notes.

## Browser and DOM

| Section | Purpose |
|---|---|
| [Browser Runtime and Web APIs](./browser-runtime-and-web-apis.md) | `window`, `document`, timers, storage, fetch, URL, and browser-provided objects. |
| [DOM and Events Patterns](./dom-and-events-patterns.md) | Querying, creating, updating, appending, removing, event listeners, and delegation. |
| [Virtual DOM Intro](./virtual-dom-intro.md) | VDOM mental model, live element creation from object descriptions, and tradeoffs. |

## Language Core

| Section | Purpose |
|---|---|
| [Types and Coercion Cheat Sheet](./types-and-coercion-cheat-sheet.md) | primitives, objects, truthiness, conversion, equality, and safe comparisons. |
| [Arrays and Objects Patterns](./arrays-and-objects-patterns.md) | array methods, parameters, mutating/non-mutating behavior, object updates, destructuring. |
| [Functions and Closures Cheat Sheet](./functions-and-closures-cheat-sheet.md) | function forms, parameters, returns, callbacks, closures, and `this`. |
| [Objects, Classes, and Prototypes Deep Dive](./objects-classes-and-prototypes-deep-dive.md) | object categories, class syntax, private fields, static methods, prototype chain, composition. |
| [Conditionals and Loops Patterns](./conditionals-and-loops-patterns.md) | `if`, `else if`, ternary, `switch`, loops, and iteration choices. |

## Async, Quality, and Debugging

| Section | Purpose |
|---|---|
| [Async and Promise Patterns](./async-and-promise-patterns.md) | promises, `async`/`await`, error boundaries, loading states, and API flows. |
| [Errors, Warnings, and Debugging Guide](./errors-warnings-and-debugging-guide.md) | runtime stack traces, syntax errors, DevTools, console, breakpoints, and triage. |
| [Commands and Tooling](./commands-and-tooling.md) | browser workflow, Node/npm tooling, test/lint/format commands. |

## Risk Notes

| Area | Watch for |
|---|---|
| DOM rendering | Use `textContent` for user data. Treat `innerHTML` as risky unless input is trusted. |
| Event handlers | Avoid scattering state changes across many handlers; update state, then render. |
| Arrays | Know whether a method mutates. `sort`, `reverse`, and `splice` are common surprises. |
| Objects | Spread is shallow. Nested objects still need nested copies. |
| Classes | JavaScript classes use prototypes. Do not assume Java/C# object semantics. |
| Async | Always handle loading, success, empty, and error states in UI code. |

---

[JavaScript](../README.md) / [Home](../../../README.md)
