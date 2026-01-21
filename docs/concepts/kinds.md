# Kinds

A **Kind** defines the static structure of a certain type of instance in Helix.

It describes what *can* exist, not what *does* exist.

A kind answers the question:

> “What kind of thing can exist?”

Kinds define **possibility**, not execution.

---

## Role of kinds

A kind specifies:
- which components are present
- whether components are mutable or immutable
- whether components are required or optional
- which marker components (tags) are possible, always present, or always absent

Kinds are purely declarative.
They do not create instances and do not contain behavior.

---

## Declaring a kind

Kinds are declared explicitly using the `#[kind]` attribute.

```rust
#[kind]
struct Player(
    Has<Id>,
    HasMut<Name>,
    Opt<Inventory>,
    OptMut<Target>,
    Is<Targettable>,
    Maybe<Promoted>,
    MaybeMut<Wounded>,
    Not<Ai>,
);
```

This declaration describes the *structure* and *capabilities* of a `Player`
instance.

---

## Components and tags in kinds

Kinds can include:
- **data components**, which may be required or optional
- **marker components (tags)**, whose presence or absence carries meaning

For data components, kinds specify:
- whether the component is always present or optional
- whether it can be accessed mutably or immutably

For marker components, kinds specify:
- tags that are always present
- tags that may or may not be present
- tags that are always absent

These rules are part of the kind’s static definition and do not require
runtime checks.

---

## Kinds and validation

Kinds are used by Helix to:
- validate queries
- determine system eligibility
- ensure that incompatible access patterns are rejected at compile time

A kind that does not satisfy a query’s requirements is simply not eligible.

---

## What kinds do not define

Kinds do not define:
- how many instances exist
- when instances are created or destroyed
- how instances are stored
- when or where systems run

Those concerns are handled by contexts and systems.

---

## Summary

- A kind defines what *can* exist
- A kind is a static, declarative structure
- A kind does not perform execution
- A kind participates in compile-time validation
