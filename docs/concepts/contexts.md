# Contexts

A **Context** defines a complete execution boundary in Helix.

Nothing exists and nothing executes outside of a context.

A context answers the question:

> “What exists here, and what can be executed here?”

---

## Role of contexts

A context is responsible for:
- defining which kinds may exist
- defining how kinds exist (cardinality)
- determining which systems are eligible
- providing explicit execution entry points
- acting as an execution and authority boundary

Contexts do not contain business logic.
They orchestrate execution.

---

## Existence vs possibility

Kinds define **what can exist**.

Contexts define **what actually exists**.

A kind may be declared globally, but it does not exist anywhere
until a context allows it to exist.

---

## Kind cardinality

When a context allows a kind to exist, it must also define **how** it exists.

Helix provides three explicit cardinality markers:

### `One<T>`

The kind exists as a **single instance**.

- the instance always exists
- it is created when the context is created
- it cannot be removed
- it can be accessed directly by systems

This is typically used for:
- core state
- controllers
- unique entities

---

### `OptOne<T>`

The kind exists as an **optional single instance**.

- at most one instance may exist
- the instance may or may not be present
- systems must handle absence explicitly
- the instance can be created or removed

This is typically used for:
- optional controllers
- transient global state
- feature-gated entities

---

### `Many<T>`

The kind exists as **multiple instances**.

- zero or more instances may exist
- instances can be created and destroyed
- systems typically iterate over them

This is the general-purpose cardinality.

---

## Declaring a context

Contexts are declared explicitly using the `#[context]` attribute.

```rust
#[context]
struct Room(
    One<Player>,
    OptOne<Boss>,
    Many<Enemy>,
);
```

This declaration defines:
- which kinds exist in the context
- their cardinality
- which instances are accessible to systems

It does not execute anything by itself.

---

## Systems and eligibility

Systems do not decide where they run.

A system declares constraints through its queries.
If those constraints are satisfied by a context, the system becomes **eligible**
for that context.

Eligibility is passive.

Being eligible does **not** imply execution.

---

## Explicit execution

A context only interacts with systems when it **explicitly executes them**.

Systems may be executed:
- from a lifecycle root
- from another system (generic or context-scoped)

If a system is never explicitly executed, it never runs,
even if it is eligible.

This ensures that execution is always intentional and visible.

---

## Lifecycle and execution roots

Helix defines a fixed set of execution roots (lifecycle phases).

A context may define entry points for these roots.
If a root is not defined, it is treated as a no-op.

Contexts do not define lifecycle phases.
They only declare how they participate in the execution model.

---

## Execution boundary

A context represents an execution boundary.

- execution does not cross contexts implicitly
- contexts may be executed in their own thread
- cross-context interactions are explicit and thread-safe

Users never manage concurrency between contexts directly.

---

## Context-scoped systems

Contexts may define systems within their own scope.

These systems:
- are bound to the context
- may access context-level APIs directly
- remain subject to the same eligibility and execution rules

This allows grouping logic around a specific execution boundary
without introducing implicit coupling.

---

## Separation of concerns

Helix enforces a strict separation between:
- **structure** (kinds)
- **access** (queries)
- **logic** (systems)
- **execution** (contexts)

Contexts are responsible only for execution and authority.

---

## Summary

- A context is a complete execution boundary
- Contexts define what exists and how it exists
- Systems become eligible based on context structure
- Execution is always explicit
- Lifecycle roots are defined by Helix, not by contexts
