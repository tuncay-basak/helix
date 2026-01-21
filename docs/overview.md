# Helix — Overview

Helix is a runtime framework built around a simple principle:

**existence, access, and execution must be explicit and statically verifiable.**

Instead of relying on implicit runtime behavior or dynamic queries, Helix uses
Rust’s type system to describe what can exist, what can interact, and where
execution is allowed to happen.

The goal is to make system structure clear, predictable, and robust by design.

---

## What Helix models

At a high level, Helix models five fundamental concepts:

- **Components** — data or tags
- **Kinds** — what kinds of instances can exist
- **Queries** — what a system intends to access or control
- **Systems** — pure logic operating over declared intent
- **Contexts** — explicit execution boundaries

Together, these concepts form a closed and explicit execution model.

---

## Components

A **Component** represents either:
- a piece of data, or
- a marker (tag) whose presence alone is meaningful.

Components are declared explicitly and attached to instances through kinds.

From the user’s perspective, data components and marker components are both
*components*, even though they differ internally in how they are represented
and accessed.

Components do not contain behavior.

---

## Kinds

A **Kind** defines a *static description* of what a certain type of instance is.

A kind specifies:
- which components are present
- whether components are mutable or immutable
- whether components are required or optional
- which tags are possible, always present or always absent

In other words, a kind answers the question:

> **“What kind of thing can exist?”**

Kinds define *possibility*, not execution.  
They do not create instances by themselves.

---

## Queries

A **Query** is a *compile-time access contract*.

It describes:
- which components or tags are required
- how they are accessed (read-only, mutable, optional, etc.)
- which structural conditions must be satisfied

Queries do not care about *how* data is stored internally.
They only express **intent and eligibility**.

Queries are used by systems to declare what they need, and by Helix to validate
that this intent is compatible with the kinds and contexts in which the system
is allowed to run.

Conceptually, queries play a role similar to:
- structural typing
- trait-based grouping
- or explicit capability declarations

---

## Systems

A **System** is pure logic.

It:
- operates only on declared queries
- has no implicit access to global state
- cannot access undeclared data

Whether a system can run depends entirely on:
- the queries it declares
- the kinds present in a context
- the context’s execution rules

Systems do not decide *when* or *where* they run.

---

## Contexts

A **Context** is a complete execution boundary.

A context defines:
- which kinds are allowed to exist
- which systems are allowed to run
- how execution is structured (e.g. lifecycle, ownership, threading)

All execution in Helix happens inside a context.

A context answers the question:

> **“What exists here, and what is allowed to execute?”**

Contexts make execution boundaries explicit and prevent accidental coupling
between unrelated parts of the system.

---

## How Helix is used

Helix is not a framework you “run”.

It is a framework you **define against**.

Users declare:
- components
- kinds
- queries
- systems
- contexts

Helix validates these declarations at compile time and provides a minimal
runtime to execute them according to the declared structure.

---

## What Helix does not do

Helix deliberately avoids:
- implicit scheduling
- hidden execution order
- global mutable state
- application-specific concerns

Those decisions are left explicit and under user control.

---

## Status

Helix is a personal project and a work in progress.

The overall model and direction are stable, while internal implementation
details continue to evolve as the design is refined.
