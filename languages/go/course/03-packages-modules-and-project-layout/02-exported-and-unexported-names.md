<h1 align="center">
    <img width="99" alt="Go logo" src="../../../../_assets/logos/go.svg">
    <br>
    <b>Go</b>
</h1>

[Home](../../../../README.md) / [Go](../../README.md) / [Chapter 03](./README.md)

---

# Exported And Unexported Names

> In Go, visibility is not controlled by `public` or `private` keywords. It is
> controlled by the first letter of the name.

**You will learn:**
- What exported means
- How uppercase and lowercase names affect package visibility
- Why exported names need comments
- How to keep public APIs smaller than internal package code
- How this rule applies to functions, types, fields, methods, constants, and variables

**Before this page, you should know:** [Packages And Imports](./01-packages-and-imports.md)

---

## The Rule

If a name starts with an uppercase letter, it is exported.

If it starts with a lowercase letter, it is unexported.

```go
func PublicMessage() string {
    return "callable from another package"
}

func privateMessage() string {
    return "only callable inside this package"
}
```

Plain language:

```text
Exported   -> other packages can use it
Unexported -> only this package can use it
```

There is no `public` keyword.
There is no `private` keyword.
The name itself carries the visibility.

---

## Package Boundary Visual

```text
package tracker

Exported:
  NewEntry
  Entry
  Entry.Topic
  Entry.Minutes

Unexported:
  validateTopic
  parseMinutes
  entryCache
```

Outside package:

```text
Can use:    tracker.NewEntry
Can use:    tracker.Entry
Cannot use: tracker.validateTopic
```

The package boundary is the wall. Capitalized names are doors through the wall.

---

## Exported Functions

```go
package tracker

import "strings"

func NewEntry(topic string, minutes int) (Entry, error) {
    topic = strings.TrimSpace(topic)
    if topic == "" {
        return Entry{}, ErrEmptyTopic
    }

    if minutes <= 0 {
        return Entry{}, ErrInvalidMinutes
    }

    return Entry{topic: topic, minutes: minutes}, nil
}

func normalizeTopic(topic string) string {
    return strings.ToLower(strings.TrimSpace(topic))
}
```

`NewEntry` is exported because callers need a safe way to create an entry.

`normalizeTopic` is unexported because it is an implementation detail.

Good public API:

```text
Call this one clear function.
It validates input.
It returns a useful result or error.
```

Weak public API:

```text
Here are seven helper functions.
Call them in the correct order and hope you remember the rules.
```

---

## Exported Types With Unexported Fields

A common Go pattern:

```go
package tracker

type Entry struct {
    topic   string
    minutes int
}

func NewEntry(topic string, minutes int) (Entry, error) {
    if topic == "" {
        return Entry{}, ErrEmptyTopic
    }
    if minutes <= 0 {
        return Entry{}, ErrInvalidMinutes
    }

    return Entry{topic: topic, minutes: minutes}, nil
}

func (e Entry) Topic() string {
    return e.topic
}

func (e Entry) Minutes() int {
    return e.minutes
}
```

`Entry` is exported. Other packages can use the type.

Its fields are unexported. Other packages cannot build invalid values directly.

Caller code:

```go
entry, err := tracker.NewEntry("Go", 45)
if err != nil {
    return err
}

fmt.Println(entry.Topic(), entry.Minutes())
```

This protects the rules:

```text
topic cannot be empty
minutes must be positive
```

---

## Exported Struct Fields

Sometimes exported fields are correct:

```go
type Config struct {
    Host string
    Port int
}
```

This is fine when:

- The type is just configuration data
- Any field value is acceptable or validated later
- You want easy JSON encoding/decoding
- The type is not protecting strict invariants

But if fields must obey rules, prefer unexported fields plus a constructor.

Decision model:

```text
Can any caller safely set this field to any value?
  yes -> exported field may be fine
  no  -> keep field unexported and provide constructor/methods
```

---

## Exported Methods

Methods follow the same rule:

```go
type Entry struct {
    topic   string
    minutes int
}

func (e Entry) Summary() string {
    return fmt.Sprintf("%s: %d minutes", e.topic, e.minutes)
}

func (e Entry) internalKey() string {
    return strings.ToLower(e.topic)
}
```

`Summary` is part of the public API.

`internalKey` is package-only.

---

## Exported Constants And Variables

Constants:

```go
const DefaultMinutes = 25
const maxTopicLength = 80
```

Variables:

```go
var ErrEmptyTopic = errors.New("topic cannot be empty")
var cache = map[string]Entry{}
```

Exported package variables are risky because other packages may mutate them.

Prefer exported errors as variables only when callers need to compare them:

```go
if errors.Is(err, tracker.ErrEmptyTopic) {
    // handle empty topic
}
```

Keep mutable package state unexported unless there is a strong reason.

---

## Comments For Exported Names

Go tooling expects exported names to have comments.

Good:

```go
// Entry records one study session.
type Entry struct {
    topic   string
    minutes int
}

// NewEntry validates topic and minutes, then returns a study entry.
func NewEntry(topic string, minutes int) (Entry, error) {
    // ...
}
```

The comment should start with the exported name:

```text
Entry ...
NewEntry ...
```

This makes generated documentation clean.

Run:

```bash
go doc ./...
```

or:

```bash
go test ./...
```

Some linters complain about missing exported comments. Even without a linter,
comments help readers.

---

## Public API Design

Ask before exporting a name:

```text
Does another package need this?
Can I explain it clearly?
Do I want to support this long-term?
Can callers misuse it?
Would a smaller exported function be better?
```

Once other code depends on an exported name, changing it becomes harder.

Beginner rule:

> Start unexported. Export only when a caller outside the package truly needs it.

---

## Mini Project: Clean Package Surface

Create a package named `tracker`.

Requirements:

- Export `Entry`
- Export `NewEntry`
- Export `ErrEmptyTopic`
- Keep fields unexported
- Keep `normalizeTopic` unexported
- Add `Topic`, `Minutes`, and `Summary` methods

Sketch:

```go
package tracker

import (
    "errors"
    "fmt"
    "strings"
)

var ErrEmptyTopic = errors.New("topic cannot be empty")

type Entry struct {
    topic   string
    minutes int
}

func NewEntry(topic string, minutes int) (Entry, error) {
    topic = normalizeTopic(topic)
    if topic == "" {
        return Entry{}, ErrEmptyTopic
    }
    if minutes <= 0 {
        return Entry{}, fmt.Errorf("minutes must be positive: %d", minutes)
    }

    return Entry{topic: topic, minutes: minutes}, nil
}

func (e Entry) Topic() string {
    return e.topic
}

func (e Entry) Minutes() int {
    return e.minutes
}

func (e Entry) Summary() string {
    return fmt.Sprintf("%s: %d minutes", e.topic, e.minutes)
}

func normalizeTopic(topic string) string {
    return strings.ToLower(strings.TrimSpace(topic))
}
```

---

## Chapter Checkpoint

You should now be able to answer:

- What makes a Go name exported?
- What makes a Go name unexported?
- Why might an exported type have unexported fields?
- When are exported struct fields okay?
- Why should exported names have comments?
- Why is exporting mutable package state risky?
- How do exported names shape your package API?

---

## Recap

- Uppercase names are exported.
- Lowercase names are unexported.
- Exported names are usable from other packages.
- Unexported names keep implementation details inside the package.
- Constructors plus unexported fields protect invariants.
- Public APIs should be small, intentional, and documented.

## Try It Yourself

Take one package from your Go practice project and list:

```text
Exported names:
Unexported helpers:
Names that should become private:
Names that need comments:
One invariant the package should protect:
```

---

[**Next ->** Structs And Methods In Go](./03-structs-and-methods-in-go.md)  
[**<- Previous** Packages And Imports](./01-packages-and-imports.md)
