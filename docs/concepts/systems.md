# Systems

A **System** represents pure logic in Helix.

A system:
- contains behavior
- declares its requirements through queries
- has no implicit access to state
- does not decide when or where it runs

At its core, a system is logic parameterized by queries.

---

## Role of systems

Systems are responsible only for:
- expressing behavior
- declaring intent through queries
- operating on eligible inputs

Systems do not:
- own state
- create or destroy instances
- manage execution
- control scheduling

Those responsibilities belong to contexts and the runtime.

---

## Systems and queries

Systems declare one or more queries as parameters.

```rust
#[system]
fn apply_velocity(entity: Movable) {
    entity.position_mut() += entity.velocity();
}
```

In this example:
- `Movable` is a query
- the system declares exactly what it needs
- no other data can be accessed

Queries define *what* a system can interact with.
Systems define *how* behavior is applied.

---

## System eligibility

A system is eligible to run only if:
- its queries are compatible with the context
- required capabilities are present

Eligibility is validated statically whenever possible.

If a system is not eligible, it cannot be executed.

---

## System scope

Systems may be defined:
- independently, as reusable logic
- inside a context, where they are bound to that context

Context-scoped systems may access context APIs directly,
without weakening isolation or introducing implicit behavior.

---

## Separation of concerns

Helix intentionally separates:
- **logic** (systems)
- **structure** (kinds)
- **access** (queries)
- **execution** (contexts)

This separation keeps systems simple and predictable,
even when used in complex execution models.

---

## Summary

- A system is pure logic
- A system declares intent through queries
- A system does not control execution
- A system is eligible only when constraints are satisfied
