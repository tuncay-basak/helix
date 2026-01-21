# Kinds

A **Kind** is a static description of what a certain type of instance can be.

Kinds define structure and possibility.
They do not create instances and do not execute logic.

---

## Role of kinds

Kinds describe:
- which **[Components](components.md)** may exist on an instance
- whether components are required or optional
- whether components are mutable or immutable
- which marker components may be present or absent

A kind answers the question:

**“What kind of thing can exist?”**

---

## Structure, not behavior

Kinds define *structure*, not *behavior*.

They do not:
- execute logic
- access data
- decide when something runs
- create or destroy instances

Behavior is implemented in **[Systems](systems.md)**.
Execution is controlled by **[Contexts](contexts.md)**.

---

## Declaration

Kinds are declared explicitly using the `#[kind]` attribute.

\`\`\`rust
#[kind]
struct Player(
    Has<Id>,
    HasMut<Position>,
    Opt<Inventory>,
    Maybe<Grounded>,
);
\`\`\`

This declaration describes which components may exist on an instance
and under which access and presence rules.

It does not create any instance.

---

## Component presence

Within a kind, components may be:
- required
- optional
- mutable or immutable

Marker components may be:
- always present
- always absent
- conditionally present

These rules are part of the kind’s static definition and are validated
at compile time whenever possible.

---

## Kinds and instances

A kind does not represent an instance.

Instances:
- are created by **[Contexts](contexts.md)**
- follow the structure defined by their kind
- are accessed by **[Systems](systems.md)** through **[Queries](queries.md)**

A kind only defines what is possible, not what exists.

---

## What kinds do not define

Kinds do not define:
- how many instances exist
- where instances exist
- how instances are iterated
- how instances are accessed at runtime

These aspects are defined by **[Contexts](contexts.md)** and
**[Queries](queries.md)**.

---

## Summary

- Kinds define structural possibilities
- Kinds describe which components may exist
- Kinds do not create instances
- Kinds do not execute logic
