# Arrays and Objects Patterns

## Mutating versus non-mutating array APIs

Mutating:
- `push`, `pop`, `shift`, `unshift`, `splice`, `sort`, `reverse`

Non-mutating:
- `map`, `filter`, `slice`, `concat`, `toSorted`, `toReversed`

Know which category you are using to avoid accidental state bugs.

## Array transforms

```javascript
const out = items
  .filter(x => x.active)
  .map(x => x.name);
```

## Find and membership patterns

```javascript
const user = users.find(u => u.id === targetId);
const hasAdmin = users.some(u => u.role === "admin");
const allValid = users.every(u => u.email);
```

## Reduce

```javascript
const total = values.reduce((sum, n) => sum + n, 0);
```

Grouping pattern:

```javascript
const byStatus = tasks.reduce((acc, task) => {
  (acc[task.status] ??= []).push(task);
  return acc;
}, {});
```

## Object updates

```javascript
const updated = { ...user, points: user.points + 1 };
```

Nested update pattern:

```javascript
const nextState = {
  ...state,
  filters: {
    ...state.filters,
    search: "new value"
  }
};
```

## Destructuring patterns

```javascript
const { id, name, ...rest } = user;
const [first, second] = items;
```

## Object utility patterns

```javascript
for (const [key, value] of Object.entries(config)) {
  console.log(key, value);
}

const keys = Object.keys(config);
const values = Object.values(config);
```

## Guidance

- Know mutating APIs (`push`, `splice`, direct assignment).
- Prefer immutable update patterns in shared state flows.
- Keep transformation pipelines readable; extract helpers when chains become dense.

---

[← JavaScript Reference](./README.md)
