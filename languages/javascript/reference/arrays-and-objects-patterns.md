# Arrays and Objects Patterns

[Home](../../../README.md) / [JavaScript](../README.md) / [Reference](./README.md)

---

> Lookup for array methods, object updates, destructuring, and state transformation patterns.

Course lesson: [Arrays, Objects, and Common Mutations](../course/02-core-language-basics/05-arrays-objects-and-common-mutations.md).

## Array Method Table

| Method | Parameters | Mutates? | Returns | Use for | Risk/tradeoff |
|---|---|---:|---|---|---|
| `push(item1, ...)` | items to append | yes | new length | add to end | changes original state |
| `pop()` | none | yes | removed item or `undefined` | remove from end | empty arrays return `undefined` |
| `shift()` | none | yes | removed item or `undefined` | remove from front | can be costly on large arrays |
| `unshift(item1, ...)` | items to prepend | yes | new length | add to front | can be costly on large arrays |
| `slice(start, end)` | start index, optional end | no | new array | copy a range | shallow copy only |
| `splice(start, deleteCount, ...items)` | index, count, inserts | yes | removed items | in-place insert/remove | easy to mutate accidentally |
| `map(callback)` | `(item, index, array)` | no | new array | transform every item | output length stays same |
| `filter(callback)` | `(item, index, array)` | no | new array | keep matching items | callback must return boolean-ish value |
| `find(callback)` | `(item, index, array)` | no | item or `undefined` | get first match | handle not found |
| `findIndex(callback)` | `(item, index, array)` | no | index or `-1` | locate position | `-1` is easy to misuse |
| `some(callback)` | `(item, index, array)` | no | boolean | any match | stops early |
| `every(callback)` | `(item, index, array)` | no | boolean | all match | empty array returns `true` |
| `reduce(callback, initial)` | `(acc, item, index, array)` | no | final value | summarize/group/build | always provide initial value |
| `includes(value)` | value to search | no | boolean | primitive membership | object comparison uses same reference |
| `sort(compareFn)` | comparison function | yes | same array | order in place | mutates; default sorts as strings |
| `toSorted(compareFn)` | comparison function | no | new sorted array | immutable sort | newer browser support |
| `reverse()` | none | yes | same array | reverse in place | mutates |
| `toReversed()` | none | no | new reversed array | immutable reverse | newer browser support |
| `join(separator)` | optional separator | no | string | display/export | converts values to strings |

## Common Examples

Add without mutation:

```javascript
const nextTasks = [...tasks, newTask];
```

Remove by id:

```javascript
const nextTasks = tasks.filter((task) => task.id !== idToRemove);
```

Toggle one item:

```javascript
const nextTasks = tasks.map((task) => {
  if (task.id !== idToToggle) return task;
  return { ...task, done: !task.done };
});
```

Total:

```javascript
const total = cart.reduce((sum, item) => sum + item.price, 0);
```

Group:

```javascript
const byRole = users.reduce((groups, user) => {
  groups[user.role] ??= [];
  groups[user.role].push(user);
  return groups;
}, {});
```

## Object Operations

| Pattern | Example | Notice |
|---|---|---|
| read property | `user.name` | use when property is known |
| dynamic read | `user[fieldName]` | use when property name is a variable |
| update copy | `{ ...user, name: "Ada" }` | shallow copy |
| remove property | `const { password, ...safeUser } = user;` | does not mutate original |
| entries | `Object.entries(config)` | useful for rendering key/value rows |
| keys | `Object.keys(config)` | property names |
| values | `Object.values(config)` | property values |
| freeze | `Object.freeze(config)` | shallow freeze only |

## Destructuring

```javascript
const { id, title, done = false } = task;
const [first, second, ...rest] = items;
```

Function parameter destructuring:

```javascript
function renderTask({ title, done }) {
  return `${done ? "[x]" : "[ ]"} ${title}`;
}
```

## Shallow Copy Warning

```javascript
const state = {
  filter: { search: "" }
};

const nextState = { ...state };
nextState.filter.search = "oops";

console.log(state.filter.search); // "oops"
```

The outer object was copied, but `filter` still points to the same nested object.

Nested copy:

```javascript
const nextState = {
  ...state,
  filter: {
    ...state.filter,
    search: "safe"
  }
};
```

## Cross References

- [DOM and Events Patterns](./dom-and-events-patterns.md)
- [Objects, Classes, and Prototypes Deep Dive](./objects-classes-and-prototypes-deep-dive.md)
- [Virtual DOM Intro](./virtual-dom-intro.md)
