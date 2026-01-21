# Helix

Helix is an experimental runtime framework written in Rust, focused on **static
correctness**, **robust execution models**, and **minimal runtime overhead**.

Its goal is to explore how far compile-time knowledge and type-driven design can be
pushed to reduce runtime checks, make invalid states unrepresentable, and enforce
explicit system boundaries.

---

## Motivation

Many existing frameworks rely heavily on runtime queries, dynamic checks, or implicit
behavior to provide flexibility. While powerful, this often makes invalid states
representable and shifts error detection to runtime.

Helix explores a different direction:
- encode execution rules and access constraints in the type system
- favor explicitness over magic
- detect structural errors as early as possible (ideally at compile time)

---

## Core concepts

Helix is built around a small set of explicit concepts:

- **Components**: pure data or marker types
- **Queries**: statically defined access contracts over components
- **Kinds**: static descriptions of entity layouts
- **Systems**: pure logic operating over explicit queries
- **Contexts**: explicit execution boundaries

These concepts are designed so that incompatible combinations fail to compile, and
runtime behavior remains explicit and predictable.

---

## Example (simplified)

```rust
#[component]
struct Position(Vec2);

#[component]
struct Velocity(Vec2);

#[query]
struct Movable(
    HasMut<Position>,
    Has<Velocity>,
);

#[system]
fn apply_velocity(entity: Movable) {
    entity.position_mut() += entity.velocity();
}
```
In this example:

- the query explicitly defines which components are accessed and how

- mutation and read-only access are distinguished at the type level

- the system logic is pure and cannot access undeclared data

Invalid access patterns or incompatible entity layouts are rejected at compile time.

Execution model
Systems do not run implicitly. They are executed inside a Context, which defines:

- which kinds of entities exist

- which systems are allowed to run

- the execution boundary (e.g. thread ownership)

This makes execution order and boundaries explicit by design.

Status
Helix is a personal project and a work in progress.
The core ideas and syntax are stable, while internal implementation details are still
being refined.

The current focus is on:

- validating the type-level model

- refining ergonomics and compile-time diagnostics

- keeping the runtime minimal and predictable

License
MIT
