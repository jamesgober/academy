# Commands and Tooling

[Home](../../../README.md) / [JavaScript](../README.md) / [Reference](./README.md)

---

> Lookup for browser workflow, local servers, Node/npm tool context, and quality commands.

## Browser Workflow

| Action | Tool |
|---|---|
| run snippets | DevTools Console |
| inspect DOM/CSS | Elements/Inspector |
| debug code | Sources/Debugger |
| inspect requests | Network |
| inspect storage | Application/Storage |

## Local Server

```bash
npx serve .
python -m http.server 5173
```

Open:

```text
http://localhost:5173
```

Use a local server for module-heavy examples, fetch examples, and realistic
browser behavior.

## Node and npm Checks

```bash
node --version
npm --version
```

Node is used here for tooling. Browser JavaScript remains the course target.

## Common npm Scripts

`package.json`:

```json
{
  "scripts": {
    "dev": "vite",
    "test": "vitest",
    "lint": "eslint .",
    "format": "prettier . --write",
    "format:check": "prettier . --check"
  }
}
```

Run:

```bash
npm run dev
npm test
npm run lint
npm run format:check
```

Exact tools can vary. The workflow should not vary: run, test, lint, format,
browser smoke-check.

## Manual Browser Smoke Check

- page loads with no Console errors
- form validation works
- add/toggle/remove behavior works
- reload keeps expected persisted data
- Network failures show visible errors
- keyboard and mouse interactions both work where relevant

## Cross References

- [Browser Runtime and Web APIs](./browser-runtime-and-web-apis.md)
- [Errors, Warnings, and Debugging Guide](./errors-warnings-and-debugging-guide.md)
- [DOM and Events Patterns](./dom-and-events-patterns.md)
