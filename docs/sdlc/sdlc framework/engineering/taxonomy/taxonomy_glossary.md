# taxonomy_glossary

Status: Normative Architecture Taxonomy
Domain: architecture
Scope: Capability boundaries and placement discipline

------

# 1. Purpose

This document defines capability boundaries used for module classification and architectural placement within the system.

The taxonomy exists to:

- prevent structural drift
- eliminate ambiguous placement decisions
- avoid creation of generic folders (for example `utils/`, `common/`, `misc/`)
- preserve architectural layering discipline

This document defines **capability segmentation within architectural layers**.

It does not define:

- coding style
- repository layout conventions
- namespace rules

Those are defined in:

```
architecture_layering
```

------

# 2. SDLC Framework Context

This document belongs to the **architecture domain** of the SDLC framework.

Architecture documents define structural constraints on software systems.

Authority relationships between SDLC domains are defined in:

```
sdlc_structure
```

Terminology used by this document is defined in:

```
sdlc_glossary
```

------

# 3. Foundation Capabilities

These capabilities typically reside under:

```
nisanijo::mythos::daedalus::foundation
```

The following capability groupings describe typical module placement.

Not all systems must implement every capability.

------

## core

Fundamental primitives used across the entire system.

Examples include:

- error model types
- memory primitives
- byte utilities
- time primitives
- low-level value abstractions

Core must not contain:

- OS interactions
- filesystem access
- UI logic
- architectural pattern frameworks

Core placement rules are defined in:

```
core_admission_and_elevation_policy
```

------

## data

Reusable data structures and domain-neutral value containers.

Examples include:

- trees
- dictionaries
- key-value stores
- structural counters
- canonical value representations

Data modules contain **structures**, not algorithms operating on them.

------

## text

Text processing and transformation utilities.

Examples include:

- encoding and decoding
- string manipulation
- lexing
- parsing
- regular expression processing
- template engines

If a module consumes, produces, or transforms text, it belongs here.

------

## io

Platform-agnostic input and output utilities.

Examples include:

- stream abstractions
- console stream formatting
- serialization mechanisms
- logger sink backends

IO modules must not perform:

- OS filesystem access
- direct syscalls

If the module models a device or stream abstraction, it belongs in `io`.

------

## diagnostics

Developer-facing observability APIs.

Examples include:

- assertions
- diagnostic reporting
- exception helpers
- source location helpers

Diagnostics owns the **diagnostic API surface**.

Logger implementations reside under:

```
foundation/io/sinks/logger
```

Diagnostics may depend on logger sinks.

Logger sinks must not depend on diagnostics.

------

## config

Configuration modeling and validation utilities.

Examples include:

- CLI argument models
- environment variable schemas
- settings schemas
- validation rules

Configuration modules must not perform runtime environment inspection.

------

## crypto

Platform-agnostic cryptographic primitives.

Examples include:

- hashing algorithms
- digest implementations

Crypto modules must not depend on:

- OS certificate stores
- platform security APIs

------

## math

Mathematical helpers and statistical calculations.

Examples include:

- statistical helpers
- numeric algorithms

Structural counters belong in `data`, not `math`.

------

## meta

Compile-time utilities.

Examples include:

- type traits
- detection idioms
- compile-time interface scaffolding

Meta modules must not introduce runtime behavior.

------

## patterns

Optional architectural pattern implementations.

Examples include:

- MVC
- observer
- producer-consumer
- facade

Patterns may depend on core.

Core must not depend on patterns.

------

## ui

User interaction constructs.

Examples include:

- controls
- prompts
- forms
- UI frameworks
- UX utilities

If the module models **user experience**, it belongs in `ui`.

If it models **device or stream behavior**, it belongs in `io`.

------

# 4. Runtime and Platform Capabilities

These capabilities belong to the infrastructure runtime portion of the system.

------

## runtime

Execution and orchestration infrastructure.

Examples include:

- application lifecycle management
- pipelines
- state machines
- resource orchestration

Runtime may depend on foundation.

Foundation must not depend on runtime.

------

## platform

Portable system-effect contracts.

Examples include:

- filesystem abstractions
- process abstractions
- console abstractions
- environment abstractions
- threading abstractions

Platform contracts must not:

- include OS headers
- perform syscalls

------

## platform_*

Platform-specific implementations.

Examples include:

- `platform_windows`
- `platform_posix`

These modules contain:

- syscalls
- OS APIs
- handle management
- error translation

Platform implementations must not leak OS-specific types into portable interfaces.

------

# 5. Prohibited Generic Folders

The following generic folders are prohibited in first-party source trees:

- `utils`
- `common`
- `misc`
- `shared`
- `helpers`
- `temp`
- `scratch`

Generic catch-all folders obscure architectural intent and make module placement ambiguous.

Capability-specific folders must be used instead.

------

# 6. Tie-Breaker Rules

If placement is unclear:

- `core` vs `data` → choose `core` only if the type is primitive and universal.
- `data` vs `math` → `data` for structure; `math` for computation.
- `io` vs `ui` → `io` for device abstraction; `ui` for user interaction.
- `foundation` vs `runtime` → choose `foundation` if no system effects are involved.

When in doubt:

Prefer **preserving architectural layering and portability**.

------

# 7. Enforcement

Taxonomy rules are enforced through architectural review.

Violations include:

- capability misplacement
- taxonomy drift
- introduction of prohibited generic folders
- violations of layer boundaries

CI validation may be introduced to detect deterministic violations.

------

# 8. References

```
architecture_layering
core_admission_and_elevation_policy
taxonomy_core_foundation
sdlc_structure
sdlc_glossary
```

