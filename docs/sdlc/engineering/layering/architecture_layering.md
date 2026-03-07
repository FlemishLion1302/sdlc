# architecture_layering

Status: Normative Architecture Specification
Domain: architecture
Scope: System layering, namespace hierarchy, and compile-time dependency rules

------

# 1. Purpose

This document defines the architectural layering model for systems developed under the SDLC framework.

It specifies:

- the canonical namespace hierarchy
- layer responsibilities
- compile-time dependency direction rules
- operating system boundary rules
- source-tree mapping expectations
- enforcement expectations

This document is the authoritative architectural specification for system layering.

The C++ Coding Standard and Engineering Guidelines must reference this document rather than duplicating its architectural rules.

Core placement rules are additionally governed by:

- Core Admission & Elevation Policy

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

# 3. Namespace Model

## 3.1 Organizational Root

```cpp
namespace nisanijo
{
}
```

- `nisanijo` is the organizational namespace.
- It prevents global collisions across products.
- It is the only namespace segment that does not map directly to a directory.

------

## 3.2 System Root

```cpp
namespace nisanijo::mythos
{
}
```

- `mythos` is the system/product root.
- All layers live under `nisanijo::mythos`.

------

## 3.3 Layer Namespaces

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

# 4. Layer Responsibilities

------

## 4.1 Layer 0 – Daedalus (Infrastructure)

Namespace:

```
nisanijo::mythos::daedalus
```

Purpose:

- Zero domain knowledge
- Infrastructure substrate for the entire system

Daedalus contains internal sublayers.

------

### 4.1.1 Internal Structure

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

### 4.1.2 core

Purpose:

- Small, stable primitives
- Portable semantic substrate
- No OS assumptions
- No policy-heavy systems

Authority: All admission, elevation, and disqualification criteria for `core`
are defined by the **Core Admission & Elevation Policy**.

Core is not demand-driven.

A component may belong in `core` even if no current `platform` contract uses it, provided it satisfies the definition of a portable semantic substrate and passes the Core Admission & Elevation Policy.

Core placement is justified by semantic stability and universality, not by current call graphs.

OS-agnostic is necessary but not sufficient for core.

OS-agnostic components that express policy, coordination, configuration, formatting strategies, routing/filtering, or other infrastructure behavior belong in `foundation` (or higher), even if they never touch OS APIs.

Decision test:

1. Is it OS-agnostic?

If no → it belongs in `platform` (contract) and `platform_*` (implementation).

1. If yes, is it a portable semantic substrate primitive that passes the Core Admission & Elevation Policy?

If yes → `core`

If no → `foundation` (or higher)

Examples include:

- `Status`
- `StatusCode`
- `Result<T>`
- invariant-bearing value types
- deterministic helpers tightly coupled to primitives
- memory/byte primitives
- policy-free string utilities
- policy-free hashing helpers

Must not contain:

- system effects
- policy-heavy infrastructure
- domain types

Specific disqualifiers are defined in the Core Admission & Elevation Policy.

------

### 4.1.3 platform

Purpose:

OS abstraction ports defining portable semantic contracts.

Characteristics:

- No OS headers
- No OS-specific types
- Interfaces only

Examples:

- monotonic clock
- filesystem port
- environment access
- process information
- threading port
- system information

Must not depend on:

- foundation
- any upper layer

------

### 4.1.4 platform_windows / platform_posix

Purpose:

Concrete OS-specific implementations of platform contracts.

Allowed:

- OS headers
- OS APIs
- OS handles
- platform-specific behavior

Must:

- translate OS semantics into platform contracts

Must not:

- depend on foundation
- depend on upper layers

OS headers must appear only here.

------

### 4.1.5 foundation

Purpose:

Reusable infrastructure and policy layer.

Built on:

- core
- platform

Contains:

- logging
- diagnostics/tracing
- configuration merging/validation
- CLI parsing
- settings systems
- serialization helpers

Policy lives here:

- formatting
- routing
- filtering
- precedence rules
- retry/drop strategies

Must not:

- depend on runtime
- depend on upper layers

------

### 4.1.6 runtime (Optional)

Purpose:

Generic hosting and lifecycle scaffolding.

Contains:

- service lifecycle
- cancellation tokens
- coordinated shutdown
- executor/scheduler abstractions
- generic host container

Must:

- remain domain-agnostic
- not include OS headers
- not contain interchange logic
- not contain domain semantics

Dependency direction:

```
runtime → foundation → platform → core
```

Foundation must not depend on runtime.

------

## 4.2 Layer 1 – Prometheus (Pure Domain)

Namespace:

```
nisanijo::mythos::prometheus
```

Purpose:

Product or problem domain logic.

Contains:

- domain types
- algorithms
- mathematical models
- scientific computation
- domain-specific logic

Must not:

- include OS headers
- depend on platform
- depend on Hermes
- depend on Hephaestus

May depend on:

```
daedalus::foundation
```

Domain logic must remain transport-agnostic and serialization-agnostic.

------

## 4.3 Layer 2 – Hephaestus (Applied Domain Composition)

Namespace:

```
nisanijo::mythos::hephaestus
```

Purpose:

Application-level domain workflows and orchestration.

Contains:

- scenario runners
- domain workflows
- application services

Must not:

- contain generic infrastructure frameworks
- include OS headers
- wrap platform

May depend on:

```
prometheus
daedalus::foundation
daedalus::runtime
hermes
```

------

## 4.4 Layer 3 – Hermes (Interchange / Boundaries)

Namespace:

```
nisanijo::mythos::hermes
```

Purpose:

Interchange and system boundary mechanisms.

Contains:

- serialization
- RPC infrastructure
- messaging
- transports
- persistence boundary logic

Must not:

- contain domain logic
- depend on Prometheus
- depend on Hephaestus

May depend on:

```
daedalus::foundation
daedalus::platform
```

Hermes defines how information moves, not what it means.

------

## 4.5 Layer 4 – Atlas (Executables)

Namespace:

```
nisanijo::mythos::atlas
```

Purpose:

Composition root and executable entry points.

Responsibilities:

- selecting platform implementations
- constructing platform services
- constructing foundation services
- constructing runtime host
- wiring Hermes and Hephaestus
- managing startup and shutdown

Atlas may depend on all layers.

Reusable logic must not accumulate here.

------

# 5. Dependency Rules (Compile-Time)

## 5.1 Internal Daedalus Direction

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

## 5.2 Inter-Layer Direction

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

## 5.3 Hard Disallows

- `daedalus::* → (prometheus|hephaestus|hermes|atlas)`
- `prometheus → daedalus::platform`
- `prometheus → hermes`
- `hermes → (prometheus|hephaestus)`
- `platform → foundation`
- `platform_* → foundation`
- any dependency that introduces cycles

------

# 6. OS Boundary Rules

OS headers and APIs may appear only under:

```
nisanijo::mythos::daedalus::platform_windows
nisanijo::mythos::daedalus::platform_posix
```

Forbidden above this boundary:

- `windows.h`
- `unistd.h`
- `pthread.h`
- raw OS handles
- OS-specific types

Portable semantics must be defined in `platform`.

------

# 7. Source Tree Mapping

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

- each `.cpp` includes its own header first
- no transitive include reliance
- namespace mirrors directory structure
- no `using namespace` in headers

------

# 8. Enforcement Expectations

Core checks enforce the Core Admission & Elevation Policy.

Foundation checks reject system effects and require platform contracts for system interactions.

Platform checks ensure contracts only and prohibit OS headers.

Platform implementations enforce OS header confinement.

### Review-Enforced

Code reviews must reject:

- upward dependencies
- OS leakage above platform layers
- cross-layer coupling violations
- cycles
- core placement violations

### CI (Optional Future Enforcement)

CI may enforce architecture via:

- include graph validation
- path-based include bans
- OS header checks
- layer cycle detection

------

# 9. Canonical Summary

- `nisanijo` is the organizational namespace
- `mythos` is the system root
- layers are:
  - Daedalus (infrastructure)
  - Prometheus (pure domain)
  - Hephaestus (application composition)
  - Hermes (interchange)
  - Atlas (executables)
- dependencies flow strictly downward
- OS code lives only in `platform_*`
- domain remains platform-agnostic and transport-agnostic
- Atlas wires the system
- Core placement is governed by the Core Admission & Elevation Policy

------

# 10. References

```
sdlc_framework_overview
taxonomy_glossary
core_admission_and_elevation_policy
```

