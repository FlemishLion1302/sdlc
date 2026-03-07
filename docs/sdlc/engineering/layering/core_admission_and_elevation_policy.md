# core_admission_and_elevation_policy

Status: Normative Architecture Policy
Domain: architecture
Scope: Admission criteria and disqualification rules for `daedalus::core`

Companion document:

```
architecture_layering
```

------

# 1. Purpose

This document defines the admission and elevation rules for components placed in:

```
nisanijo::mythos::daedalus::core
```

It specifies:

- what qualifies for placement in `core`
- what disqualifies a component from `core`
- when components should instead reside in `platform`, `platform_*`, or `foundation`

The goals of this policy are to:

- prevent uncontrolled growth of `core`
- preserve a small, stable semantic substrate
- make placement decisions deterministic
- enable consistent architectural review

Core is not a convenience location for reusable code.

Core is the **portable semantic substrate** of the system.

------

# 2. SDLC Framework Context

This document belongs to the **architecture domain** of the SDLC framework.

Architecture documents define structural constraints on software systems and take precedence over standards and guidelines when conflicts arise.

The authority relationships between SDLC domains are defined in:

```
sdlc_structure
```

Terminology used by this document is defined in:

```
sdlc_glossary
```

------

# 3. Definition of Core

Core contains:

> Fundamental value semantics and minimal invariant-preserving behavior that are universally safe for all layers to depend on.

Core components are:

- platform-agnostic
- policy-free
- effect-free
- low churn
- semantically stable

Core is not:

- infrastructure
- configuration systems
- orchestration
- strategy logic
- system-effect abstraction
- a general-purpose utility collection

------

# 4. Core Promotion Test (All Criteria Required)

A component may be placed in `core` only if **all criteria below are satisfied**.

------

## 4.1 Cross-Layer Necessity

The component is required by multiple architectural layers.

Typical examples include:

- types shared between platform contracts and foundation
- value semantics used throughout the system

If a component is used only by foundation, it likely belongs in `foundation`.

------

## 4.2 Vocabulary, Not Policy

The component represents:

- a value model
- a primitive concept
- a stable invariant

It must not represent:

- configuration
- defaults
- strategy selection
- precedence rules
- product decisions
- behavioral policies

Core defines **vocabulary**, not behavior policy.

------

## 4.3 Deterministic Behavior

All behavior must be:

- deterministic
- dependent only on explicit input parameters
- free of hidden state
- free of global access

Permitted behavior includes:

- invariant enforcement
- local value transformations
- comparison
- hashing
- canonical textual formatting or parsing

------

## 4.4 Effect-Free

If a component must consult the machine to function,
it does not belong in core.

Disallowed operations include:

- clocks (`now()`)
- sleeping
- filesystem access
- environment variables
- process inspection
- threading primitives
- OS handles
- syscalls
- OS error translation

System effects belong in:

```
platform       (contracts)
platform_*     (implementations)
```

------

## 4.5 Stability Commitment

Components placed in core are treated as **low-churn infrastructure**.

They must:

- maintain strong backward compatibility
- remain suitable as long-term dependency surfaces
- avoid frequent semantic change

If high churn is expected, the component does not belong in core.

------

# 5. Core Disqualifiers

A component must **not** be placed in core if any of the following apply.

------

## 5.1 System-Effect Marker

If the component includes:

- clocks or timers
- filesystem operations
- environment access
- process inspection
- virtual memory mapping
- OS-specific headers or types

It belongs in:

```
platform
platform_*
```

------

## 5.2 Policy Marker

If the component introduces:

- configuration management
- default value selection
- strategy objects
- logging frameworks
- rule engines
- validation pipelines
- feature flags

It belongs in:

```
foundation
```

------

## 5.3 Orchestration Marker

If the component coordinates subsystems or workflows, such as:

- execution state machines
- pipelines
- schedulers
- service managers

It belongs in:

```
runtime
foundation
```

------

## 5.4 Dependency Gravity

If inclusion of the component:

- introduces heavy dependencies
- creates layering violations
- requires linking against infrastructure

then it must not reside in core.

------

# 6. Time Domain Split (Canonical Example)

This example illustrates how time-related functionality is distributed across layers.

### Core

```
Duration
Timestamp
TimeSpan
```

Includes:

- arithmetic on time values
- deterministic formatting and parsing

------

### Platform

```
clock_service
timer_service
sleep_for
```

Defines portable clock interfaces.

------

### platform_*

Contains OS-specific implementations such as:

- system clock calls
- OS timer handles
- platform time APIs

------

### Foundation

Contains policy logic built on time primitives, such as:

- scheduling policies
- retry and backoff logic
- logging timestamp formatting policies

------

# 7. Memory Domain Split (Canonical Example)

### Core

```
Byte
ByteSpan
BufferView
```

Includes:

- bit operations
- alignment helpers
- deterministic layout helpers

------

### Platform

Defines memory abstraction contracts such as:

```
virtual_memory_service
shared_memory_service
```

------

### platform_*

Contains OS-specific implementations such as:

- `VirtualAlloc`
- `mmap`
- page mapping handles
- page size queries

------

### Foundation

Contains memory policy mechanisms including:

- memory pools
- allocation policies
- caching strategies
- allocation diagnostics

------

# 8. Algorithm Placement Rule

Algorithms belong in core **only if** they:

- operate on core value types
- are deterministic
- are policy-free
- express intrinsic semantics of the type

Examples include:

- equality
- ordering
- hashing
- canonical transformations

Algorithms do **not** belong in core if they are:

- parsing frameworks
- regex engines
- generic container algorithms
- state machine engines
- scheduling logic

These belong in `foundation` or `runtime`.

------

# 9. Asymmetric Decision Principle

Promotion to core requires **positive proof**.

All promotion criteria must be satisfied.

Rejection requires only **one disqualifying signal**.

When uncertainty exists, the default placement is:

```
foundation
```

------

# 10. Summary

Core represents:

- the semantic substrate of the system
- the vocabulary shared across architectural layers
- stable, deterministic value semantics

Platform models system effects.

Foundation builds infrastructure and policy on top of core and platform.

This separation preserves:

- architectural clarity
- portability
- deterministic review enforcement
- long-term system stability

------

# 11. References

```
architecture_layering
sdlc_framework_overview
taxonomy_glossary
```

