<h1 align="center">
    <img width="99" alt="JavaScript logo" src="../../../../_assets/logos/js.svg">
    <br>
    <b>JavaScript</b>
</h1>

[Home](../../../../README.md) / [JavaScript](../../README.md) / [Chapter 05](./README.md)

---

# Capstone: Task Tracker Web App

> Build a realistic frontend project combining modules, data modeling, DOM rendering, event handling, and browser persistence.

**You will learn:**
- practical frontend project structure
- UI/domain separation
- delivery criteria for a complete small product

**Before this page, you should know:** all previous chapters.

---

## Suggested structure

```text
task-tracker/
├── index.html
├── styles.css
├── src/
│   ├── app.js
│   ├── taskService.js
│   ├── domRenderer.js
│   └── storage.js
└── tests/
    └── taskService.test.js
```

## Required features

- add task
- list tasks
- mark task complete
- persist tasks to browser storage
- validate and report user input errors

Optional stretch:
- filter tasks by status
- show task counts and completion progress

## Milestones

1. define task model and service APIs
2. implement DOM renderer and event wiring
3. implement browser storage persistence
4. add tests for task state transitions
5. run lint/test loop and document constraints

## Expected output examples

- Task appears immediately in UI after submit
- Completed task visually marked in list
- Reload restores persisted tasks

---

## Recap

- Small frontend apps stay maintainable through clear module boundaries.
- DOM rendering and events should stay separated from business logic.
- Tests and linting are mandatory finishing steps.

## Try it yourself

Implement one vertical slice: add a task, render it to the page, and persist it to browser storage.

---

[**Next ->** Chapter 05 Final Checkpoint](./05-chapter-05-final-checkpoint.md)  
[**<- Previous** Linting, Formatting, and Quality Gates](./03-linting-formatting-and-quality-gates.md)
