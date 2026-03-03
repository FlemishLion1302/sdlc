# Core Admission & Elevation Policy

**Status:** Authoritative (Review-Enforced)
**Applies To:** Architectural Layering Model
**Companion To:** architecture_layering.md

------

# 1. Purpose

This document formalizes:

- What qualifies for placement in **core**
- What disqualifies a component from **core**
- When components should instead reside in **platform**, **platform_\***, or **foundation**

The goal is to:

- Prevent core creep
- Preserve a small, stable substrate
- Make placement decisions deterministic
- Enable review-enforced consistency

Core is not a convenience bucket.
Core is the **portable semantic substrate** of the system.

------

# 2. Definition of Core

Core contains:

> Fundamental value semantics and minimal invariant-preserving behavior
> that are universally safe for all layers to depend on.

Core is:

- Platform-agnostic
- Policy-free
- Effect-free
- Low-churn
- Semantically stable

Core is not:

- Infrastructure
- Configuration
- Orchestration
- Strategy
- System-effect abstraction
- Utility aggregation

------

# 3. Core Promotion Test (All Criteria Required)

A component MAY be placed in `core` only if **all** conditions below are satisfied.

## 3.1 Cross-Layer Necessity

The component is required by multiple layers
(e.g., platform contracts and foundation both depend on it).

If it is used only by foundation, it likely belongs in foundation.

------

## 3.2 Vocabulary, Not Policy

The component represents:

- A value model
- A primitive concept
- A stable invariant

It does NOT represent:

- Defaults
- Configuration
- Strategies
- Precedence rules
- Product decisions
- Behavioral policies

------

## 3.3 Deterministic Behavior

All behavior must be:

- Purely deterministic
- Dependent only on input parameters
- Free of hidden state
- Free of global access

Permitted behavior:

- Invariant enforcement
- Local value transformations
- Comparison
- Hashing
- Canonical formatting/parsing (if purely textual)

------

## 3.4 Effect-Free

If a component must consult the machine to function,
it is not core.

Disallowed in core:

- Clocks (`now()`)
- Sleeping
- Filesystem access
- Environment variables
- Process APIs
- Threading primitives
- OS handles
- Syscalls
- OS error translation

System effects belong to:

- `platform` (contracts)
- `platform_*` (implementations)

------

## 3.5 Stability Commitment

If placed in core, the component:

- Is treated as low-churn
- Has strong backward-compatibility expectations
- Is suitable as a long-term dependency surface

If high churn is expected, it does not belong in core.

------

# 4. Core Disqualifiers (Any One Is Sufficient)

A component MUST NOT be placed in core if it exhibits any of the following.

## 4.1 System-Effect Marker

Contains:

- `now`, `sleep`, `timer`
- Filesystem operations
- Process or environment inspection
- Virtual memory reservation/mapping
- OS-specific headers or types

→ Move to `platform` or `platform_*`.

------

## 4.2 Policy Marker

Contains:

- Configuration management
- Default value selection
- Strategy objects
- Logging frameworks
- Rule engines
- Validation pipelines
- Feature flags

→ Move to `foundation`.

------

## 4.3 Orchestration Marker

Coordinates multiple subsystems or workflows:

- State machines (execution-level)
- Pipelines
- Schedulers
- Managers that coordinate services

→ Move to `runtime` or `foundation/patterns`.

------

## 4.4 Dependency Gravity

If inclusion of the component:

- Pulls in heavy dependencies
- Introduces layering violations
- Requires linking against infrastructure

It does not belong in core.

------

# 5. Time Domain Split (Canonical Example)

## Core

- `Duration`
- `Timestamp`
- `TimeSpan`
- Arithmetic on time values
- Pure formatting/parsing

## Platform

- `clock_service`
- `timer_service`
- `sleep_for`
- Portable clock interfaces

## platform_*

- OS time APIs
- System clock calls
- Timer handles
- OS error translation

## Foundation

- Scheduling policies
- Retry/backoff logic
- Logging timestamp formatting policies

------

# 6. Memory Domain Split (Canonical Example)

## Core

- `Byte`
- `ByteSpan`
- `BufferView`
- Bit operations
- Alignment helpers
- Deterministic layout helpers

## Platform

- `virtual_memory_service`
- `shared_memory_service`
- Memory mapping interfaces
- Page protection enums

## platform_*

- `VirtualAlloc`
- `mmap`
- OS mapping handles
- Page size queries

## Foundation

- Memory pools
- Allocation policies
- Caching strategies
- Allocation diagnostics

------

# 7. Algorithm Placement Rule

Algorithms belong in core ONLY if:

- They operate on core value types
- They are deterministic and policy-free
- They express intrinsic semantics of the type

Examples:

- Equality
- Ordering
- Hash computation
- Canonical transformation

Algorithms DO NOT belong in core if they are:

- Parsing frameworks
- Regex engines
- Generic container algorithms
- State machine engines
- Scheduling logic

Those belong in foundation or runtime.

------

# 8. Asymmetric Decision Principle

Promotion to core requires **positive proof** (all promotion criteria satisfied).

Rejection from core requires only **one red flag**.

When uncertain → default to foundation.

------

# 9. Summary

Core is:

- The semantic substrate
- The vocabulary of the system
- Stable, effect-free, policy-free
- Minimal but powerful

Platform models system effects.

Foundation builds policy and infrastructure on top of core and platform.

This separation preserves:

- Layer clarity
- Portability
- Review enforceability
- Long-term architectural stability

------

# End of Document