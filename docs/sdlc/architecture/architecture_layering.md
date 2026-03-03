# Architecture Layering and Dependency Rules

## 1. Scope and Intent

This document defines:

- The canonical namespace hierarchy
- Layer responsibilities
- Compile-time dependency direction rules
- OS boundary rules
- Source-tree mapping
- Enforcement expectations

This is the **authoritative source of truth** for architectural layering.

The C++ Coding Standard and Engineering Guidelines must reference this document rather than duplicating its rules.

Core placement rules are additionally governed by:

- **Core Admission & Elevation Policy** (companion authority for `daedalus::core`)

------

# 2. Namespace Model

## 2.1 Organizational Root

```cpp
namespace nisanijo
{
}
```

- `nisanijo` is the organizational namespace.
- It prevents global collisions across products.
- It is the only namespace segment that does not map directly to a directory.

------

## 2.2 System Root

```cpp
namespace nisanijo::mythos
{
}
```

- `mythos` is the system/product root.
- All layers live under `nisanijo::mythos`.

------

## 2.3 Layer Namespaces

```cpp
namespace nisanijo::mythos
{
namespace daedalus   { } // Layer 0 – Infrastructure
namespace prometheus { } // Layer 1 – Pure domain
namespace hephaestus { } // Layer 2 – Applied domain composition
namespace hermes     { } // Layer 3 – Interchange/boundaries
namespace atlas      { } // Layer 4 – Executables
}
```

These five namespaces define the architectural layers.

`nisanijo` is not a layer.

------

# 3. Layer Responsibilities

------

## 3.1 Layer 0 – Daedalus (Infrastructure)

Namespace:

```
nisanijo::mythos::daedalus
```

Purpose:

- Zero domain knowledge
- Infrastructure substrate for the entire system

Daedalus contains internal sublayers.

### 3.1.1 Internal Structure

```cpp
namespace nisanijo::mythos::daedalus
{
namespace core            { }
namespace platform         { }
namespace platform_windows { }
namespace platform_posix   { }
namespace foundation       { }
namespace runtime          { } // optional
}
```

------

### 3.1.2 `core`

Purpose:

- Small, stable primitives
- Portable semantic substrate
- No OS assumptions
- No policy-heavy systems

**Authority:** All admission, elevation, and disqualification criteria for `core`
are defined by the **Core Admission & Elevation Policy**.

**Core is not demand-driven.**
 A component may belong in `core` even if no current `platform` contract uses it, provided it satisfies the definition of a portable semantic substrate and passes the Core Admission & Elevation Policy. Core placement is justified by semantic stability and universality, not by current call graphs.

**OS-agnostic is necessary but not sufficient for core.**
 OS-agnostic components that express policy, coordination, configuration, formatting strategies, routing/filtering, or other infrastructure behavior belong in `foundation` (or higher), even if they never touch OS APIs.

**Decision test (core vs platform vs foundation):**

1) Is it OS-agnostic?
   - If **no** → it belongs in `platform` (contract) and `platform_*` (implementation).
2) If **yes**, is it a portable semantic substrate primitive that passes the Core Admission & Elevation Policy
   (effect-free, policy-free, domain-free, invariant-bearing value type or tightly coupled helper)?
   - If **yes** → `core`
   - If **no** → `foundation` (or higher)

This section provides orientation and examples only.

Contains (examples):

- `Status`, `StatusCode`, `Result<T>`
- Low-level primitives and invariant-bearing value types
- Deterministic helpers tightly coupled to those primitives
- Memory/byte primitives (policy-free)
- String utilities (policy-free)
- Hash utilities (policy-free)

- Must not contain:

  - Any system effects
  - Any policy-heavy infrastructure
  - Any domain types

  Specific disqualifiers and red-flag markers are defined in the Core Admission & Elevation Policy.

------

### 3.1.3 `platform`

Purpose:

- OS abstraction ports
- Portable semantic contracts

Characteristics:

- No OS headers
- No OS-specific types
- Interfaces only

Examples:

- Monotonic clock
- Filesystem port
- Environment access
- Process information
- Threading port
- System information

Must not depend on:

- `foundation`
- Any upper layer

------

### 3.1.4 `platform_windows` / `platform_posix`

Purpose:

- Concrete OS-specific implementations of `platform`

Allowed:

- OS headers
- OS APIs
- OS handles
- Platform-specific behavior

Must:

- Translate OS semantics to `platform` contracts

Must not:

- Depend on `foundation`
- Depend on upper layers

OS headers must appear only here.

------

### 3.1.5 `foundation`

Purpose:

- Reusable infrastructure
- Policy layer

Built on:

- `core`
- `platform`

Contains:

- Logging
- Diagnostics/tracing
- Configuration merging/validation
- CLI parsing
- Settings systems
- Serialization helpers (mechanism support only)

Policy lives here:

- Formatting
- Routing
- Filtering
- Precedence rules
- Retry/drop strategies

Must not:

- Depend on runtime
- Depend on upper layers

------

### 3.1.6 `runtime` (Optional)

Purpose:

- Generic hosting and lifecycle scaffolding

Contains:

- Service lifecycle (`start/stop`)
- Cancellation tokens
- Coordinated shutdown
- Executor/scheduler abstractions
- Generic host container

Must:

- Remain domain-agnostic
- Not include OS headers
- Not contain interchange logic (Hermes)
- Not contain domain semantics

Dependency direction:

```
runtime → foundation → platform → core
```

Foundation must not depend on runtime.

------

## 3.2 Layer 1 – Prometheus (Pure Domain)

Namespace:

```
nisanijo::mythos::prometheus
```

Purpose:

- Product/problem domain logic

Contains:

- Math
- Physics
- Scientific models
- Domain types
- Algorithms

Must not:

- Include OS headers
- Depend on `platform`
- Depend on Hermes
- Depend on Hephaestus

May depend on:

- `daedalus::foundation`

Domain must remain serialization-agnostic and transport-agnostic.

------

## 3.3 Layer 2 – Hephaestus (Applied Domain Composition)

Namespace:

```
nisanijo::mythos::hephaestus
```

Purpose:

- Application-level domain workflows
- Use-case orchestration
- Domain composition

Contains:

- Scenario runners
- Domain workflows
- Application services (DDD sense)

Must not:

- Contain generic system frameworks
- Include OS headers
- Rewrap platform

May depend on:

- `prometheus`
- `daedalus::foundation`
- `daedalus::runtime`
- `hermes`

------

## 3.4 Layer 3 – Hermes (Interchange / Boundaries)

Namespace:

```
nisanijo::mythos::hermes
```

Purpose:

- Interchange and boundary mechanisms

Contains:

- Serialization mechanism
- RPC infrastructure
- Messaging
- Transports
- Persistence boundary logic

Must not:

- Contain domain logic
- Depend on Prometheus
- Depend on Hephaestus

May depend on:

- `daedalus::foundation`
- `daedalus::platform`

Hermes defines how information moves, not what it means.

------

## 3.5 Layer 4 – Atlas (Executables)

Namespace:

```
nisanijo::mythos::atlas
```

Purpose:

- Composition root
- Entry points
- Tools and applications

Responsibilities:

- Select `platform_windows` or `platform_posix`
- Construct platform implementations
- Construct foundation services
- Construct runtime host (if used)
- Wire Hermes and Hephaestus
- Own startup/shutdown

Atlas may depend on all layers.

Reusable logic must not accumulate here.

------

# 4. Dependency Rules (Compile-Time)

## 4.1 Internal Daedalus Direction

Allowed:

```
runtime    → foundation → platform → core
platform_* → platform   → core
```

Not allowed:

```
platform → foundation
platform_* → foundation
foundation → runtime
any cycles
```

------

## 4.2 Inter-Layer Direction

Allowed:

```
atlas       → hephaestus
atlas       → hermes
atlas       → prometheus
atlas       → daedalus::*

hephaestus  → prometheus
hephaestus  → hermes
hephaestus  → daedalus::foundation
hephaestus  → daedalus::runtime

prometheus  → daedalus::foundation

hermes      → daedalus::foundation
hermes      → daedalus::platform
```

------

## 4.3 Hard Disallows

- `daedalus::* → (prometheus|hephaestus|hermes|atlas)`
- `prometheus → daedalus::platform`
- `prometheus → hermes`
- `hermes → (prometheus|hephaestus)`
- `platform → foundation`
- `platform_* → foundation`
- Any dependency that introduces cycles

------

# 5. OS Boundary Rules

OS headers/APIs may appear only under:

```
nisanijo::mythos::daedalus::platform_windows
nisanijo::mythos::daedalus::platform_posix
```

Forbidden above this boundary:

- `windows.h`
- `unistd.h`
- `pthread.h`
- Raw OS handles
- OS-specific types

Portable semantics must be defined in `platform`.

------

# 6. Source Tree Mapping

Directory mapping mirrors namespaces.

```
include/nisanijo/mythos/
  daedalus/
    core/
    platform/
    foundation/
    runtime/
  prometheus/
  hephaestus/
  hermes/
  atlas/

src/nisanijo/mythos/
  daedalus/
    core/
    platform/
    platform_windows/
    platform_posix/
    foundation/
    runtime/
  prometheus/
  hephaestus/
  hermes/
  atlas/
```

Rules:

- Each `.cpp` includes its own header first.
- No transitive include reliance.
- Namespace mirrors directory structure.
- No `using namespace` in headers.

------

# 7. Enforcement

**Core checks:** enforce Core Admission & Elevation Policy.

**Foundation checks:** reject system effects; require platform contracts for system interactions.

**Platform checks:** contracts only; no OS headers/types; no foundation dependency.

**platform_\* checks:** OS headers only here; translate OS semantics.

## 7.1 Review-Enforced

Reviewers must reject:

- Upward dependencies
- OS leakage above platform_*
- Cross-layer coupling violations
- Cycles
- Core violations as defined by the Core Admission & Elevation Policy

## 7.2 CI (Optional Future Enforcement)

May enforce via:

- Include graph validation
- Path-based include bans
- OS header checks
- Layer cycle detection

------

# 8. Canonical Summary

- `nisanijo` is the organizational namespace.
- `mythos` is the system root.
- Layers are:
  - Daedalus (infrastructure)
  - Prometheus (pure domain)
  - Hephaestus (applied domain)
  - Hermes (interchange)
  - Atlas (executables)
- Dependencies flow strictly downward.
- OS code lives only in `platform_*`.
- Domain remains platform-agnostic and transport-agnostic.
- Atlas wires everything.
- Core placement is governed by the Core Admission & Elevation Policy.

------

# Appendix A – Dependency Arrow Semantics

This appendix defines the precise meaning of the dependency arrows used
throughout this document.

------

## A.1 Definition

In this document, an arrow of the form:

```
A → B
```

means:

> A is permitted to have a **compile-time dependency** on B.

This is a build-time rule.

It governs include relationships, symbol visibility, and type usage.

------

## A.2 What “Compile-Time Dependency” Means

A compile-time dependency includes:

- Including headers from B
- Referencing types defined in B
- Inheriting from interfaces defined in B
- Instantiating templates defined in B
- Calling inline functions defined in B
- Depending on constants, enums, or traits defined in B

In practical terms:

If code in A requires `#include <nisanijo/mythos/B/...>`,
then A depends on B.

------

## A.3 What the Arrow Does *Not* Mean

The arrow does not imply:

- Required usage
- Required construction
- Required linking
- Required runtime calls

It does not describe runtime control flow.

It does not describe ownership.

It does not describe object lifetime relationships.

It only describes allowed build-time dependency direction.

------

## A.4 Runtime Control Flow Is Irrelevant to Layering

Runtime calls may flow in any direction as long as
compile-time dependencies remain valid.

For example:

- Atlas constructs Hephaestus
- Hephaestus calls Foundation
- Foundation calls Platform
- Platform calls OS APIs
- OS callbacks may invoke code higher in the stack

This does not violate layering if compile-time edges remain downward.

Architecture is enforced at build-time, not at runtime.

------

## A.5 Dependency Inversion Clarification

Dependency inversion does not change the direction of compile-time dependency.

If an interface is defined in layer B,
and implemented in layer A,
then A depends on B.

If an interface is defined in layer A,
and implemented in layer B,
then B depends on A.

Interface placement therefore determines dependency direction.

To preserve layering:

- Interfaces for lower-level services must be defined in lower layers.
- Upper layers must not define interfaces implemented by lower layers.

------

## A.6 Prohibited Upward Dependency

If the architecture states:

```
prometheus → daedalus::foundation
```

then the following are forbidden:

- `daedalus::foundation` including headers from `prometheus`
- `daedalus::foundation` referencing types defined in `prometheus`
- Any transitive include that introduces such a dependency

No layer may depend on a layer above it.

------

## A.7 Why Compile-Time Direction Is the Enforced Semantic

Compile-time dependency direction:

- Is statically verifiable
- Is CI-enforceable
- Prevents cyclic coupling
- Preserves architectural stability
- Ensures refactor safety

Runtime call graphs are not used to define architecture boundaries.

------

## A.8 Summary

Throughout this document:

```
A → B
```

means:

> A may include and reference B at compile-time.
> B must not include or reference A.

All arrows define allowed build-time dependency direction only.

