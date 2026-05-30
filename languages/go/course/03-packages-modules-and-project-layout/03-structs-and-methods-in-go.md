<h1 align="center">
    <img width="99" alt="Go logo" src="../../../../_assets/logos/go.svg">
    <br>
    <b>Go</b>
</h1>

[Home](../../../../README.md) / [Go](../../README.md) / [Chapter 03](./README.md)

---

# Structs And Methods In Go

> Structs group related data. Methods attach behavior to a type without turning
> Go into a class-heavy language.

**You will learn:**
- How to define structs
- How to initialize structs safely
- How value and pointer receivers differ
- How methods help keep behavior near data
- How struct tags support JSON and other tools
- How to avoid turning structs into public bags of invalid state

**Before this page, you should know:** [Exported And Unexported Names](./02-exported-and-unexported-names.md)

---

## Structs Group Data

```go
type Entry struct {
    Topic   string
    Minutes int
    Notes   string
}
```

An `Entry` value contains three fields.

Create one:

```go
entry := Entry{
    Topic:   "Go",
    Minutes: 45,
    Notes:   "packages and methods",
}
```

Prefer named fields. This is readable and safer when fields are reordered.

Avoid positional literals for exported structs:

```go
entry := Entry{"Go", 45, "packages and methods"}
```

That works, but it asks the reader to memorize field order.

---

## Zero Values

Every Go type has a zero value.

For the struct above:

```go
var entry Entry
```

Means:

```text
Topic   = ""
Minutes = 0
Notes   = ""
```

Zero values can be useful. But sometimes a zero-value struct is not valid for
your domain.

If an entry must have a topic and positive minutes, use a constructor function.

---

## Constructor Function Pattern

Go does not have constructors as a special language feature.

It uses normal functions by convention:

```go
func NewEntry(topic string, minutes int, notes string) (Entry, error) {
    topic = strings.TrimSpace(topic)
    notes = strings.TrimSpace(notes)

    if topic == "" {
        return Entry{}, errors.New("topic cannot be empty")
    }
    if minutes <= 0 {
        return Entry{}, fmt.Errorf("minutes must be positive: %d", minutes)
    }

    return Entry{
        Topic:   topic,
        Minutes: minutes,
        Notes:   notes,
    }, nil
}
```

Caller:

```go
entry, err := NewEntry("Go", 45, "methods")
if err != nil {
    return err
}
```

This pattern is common:

```text
NewTypeName(...) (TypeName, error)
```

---

## Methods

A method is a function with a receiver.

```go
func (e Entry) Summary() string {
    return fmt.Sprintf("%s: %d minutes", e.Topic, e.Minutes)
}
```

Receiver:

```go
(e Entry)
```

Plain meaning:

```text
This function belongs to Entry values.
Inside the function, call the current Entry e.
```

Call:

```go
fmt.Println(entry.Summary())
```

---

## Value Receiver

This method receives a copy of the value:

```go
func (e Entry) Summary() string {
    return fmt.Sprintf("%s: %d minutes", e.Topic, e.Minutes)
}
```

Use value receivers when:

- The method does not modify the value
- The struct is small
- Copying is cheap
- You want value-like behavior

Value receiver cannot modify the caller's original value:

```go
func (e Entry) AddMinutes(minutes int) {
    e.Minutes += minutes
}
```

This changes only the copy.

---

## Pointer Receiver

This method can modify the original value:

```go
func (e *Entry) AddMinutes(minutes int) error {
    if minutes <= 0 {
        return fmt.Errorf("minutes must be positive: %d", minutes)
    }

    e.Minutes += minutes
    return nil
}
```

Use pointer receivers when:

- The method modifies the value
- The struct is large and copying is expensive
- The type contains synchronization fields
- You want consistency across methods

Caller:

```go
entry := Entry{Topic: "Go", Minutes: 45}
err := entry.AddMinutes(15)
```

Go automatically takes the address when it can.

---

## Receiver Naming

Use short receiver names:

```go
func (e Entry) Summary() string
func (s Store) Save(entry Entry) error
func (c Client) Fetch(id string) (Entry, error)
```

Avoid:

```go
func (this Entry) Summary() string
func (self Entry) Summary() string
```

Go style does not use `this` or `self`.

---

## Unexported Fields With Accessor Methods

If a struct protects rules, keep fields unexported:

```go
type Entry struct {
    topic   string
    minutes int
}

func (e Entry) Topic() string {
    return e.topic
}

func (e Entry) Minutes() int {
    return e.minutes
}
```

This prevents invalid direct construction from another package:

```go
// impossible outside package:
Entry{topic: "", minutes: -10}
```

Use this when correctness matters more than raw convenience.

---

## Struct Tags

Struct tags attach metadata to fields.

Common with JSON:

```go
type EntryDTO struct {
    Topic   string `json:"topic"`
    Minutes int    `json:"minutes"`
    Notes   string `json:"notes,omitempty"`
}
```

`json:"topic"` means JSON uses the field name `topic`.

`omitempty` means the field can be left out when it has a zero value.

Tags are strings read by packages such as `encoding/json`.

Do not put business rules only in tags. Validate data in code too.

---

## Methods Versus Functions

Use a method when behavior naturally belongs to one type:

```go
entry.Summary()
entry.AddMinutes(15)
```

Use a function when behavior combines several values or does not belong to one
receiver:

```go
TotalMinutes(entries)
ParseEntry(line)
```

There is no prize for making everything a method.

---

## Mini Project: Study Entry Type

Build this type:

```go
type Entry struct {
    topic   string
    minutes int
    notes   string
}
```

Add:

- `NewEntry(topic string, minutes int, notes string) (Entry, error)`
- `Topic() string`
- `Minutes() int`
- `Notes() string`
- `Summary() string`
- `AddMinutes(minutes int) error`

Test these cases:

- Empty topic returns error
- Zero minutes returns error
- Summary includes topic and minutes
- `AddMinutes` changes the original value
- Negative added minutes returns error

---

## Chapter Checkpoint

You should now be able to answer:

- What does a struct do?
- Why are named struct literals usually clearer?
- What is a zero value?
- Why does Go use `NewType` functions?
- What is a method receiver?
- When should a method use a pointer receiver?
- What are struct tags commonly used for?
- When is a function better than a method?

---

## Recap

- Structs group fields into one type.
- Go constructor functions are normal functions by convention.
- Methods attach behavior to a type.
- Value receivers get a copy.
- Pointer receivers can modify the original value.
- Unexported fields plus constructors protect invariants.
- Struct tags support tools such as JSON encoding.

## Try It Yourself

Create a `Task` struct with unexported fields, a `NewTask` constructor, accessor
methods, and a pointer receiver method named `Complete`.

---

[**Next ->** Interfaces In Plain Language](./04-interfaces-in-plain-language.md)  
[**<- Previous** Exported And Unexported Names](./02-exported-and-unexported-names.md)
