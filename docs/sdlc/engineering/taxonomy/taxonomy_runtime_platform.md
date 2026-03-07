# taxonomy_runtime_platform

Status: Normative Architecture Taxonomy
Domain: architecture
Scope: Runtime-layer and platform capability segmentation

------

# 1. Purpose

This document defines the capability taxonomy for the runtime and platform portions of the infrastructure layer.

It describes how execution infrastructure and system-effect abstractions are segmented within the system.

Conceptually:

```
runtime          = execution model and orchestration
platform         = portable contracts for system effects
platform_*       = OS-specific implementations
```

This taxonomy exists to:

- maintain deterministic placement of runtime capabilities
- preserve clear separation between orchestration and system effects
- enforce strict OS boundary containment

This document does not define coding style or repository layout rules.

Coding conventions are governed by the C++ Coding Standard.

Source-tree mapping and layer structure are defined in:

```
architecture_layering
```

------

# 2. SDLC Framework Context

This document belongs to the **architecture domain** of the SDLC framework.

Architecture documents define structural constraints on software systems.

Authority relationships between SDLC domains are defined in:

```
sdlc_framework_overview
```

Terminology used in this document is defined in:

```
taxonomy_glossary
```

------

# 3. Infrastructure Layer Context

Runtime and platform capabilities reside within:

```
nisanijo::mythos::daedalus
```

Conceptually:

```
runtime      → foundation → platform → core
platform_*   → platform
```

Runtime orchestrates execution.

Foundation provides reusable infrastructure primitives.

Platform defines portable system-effect contracts.

Platform implementations translate OS semantics into portable contracts.

Foundation must not depend on runtime.

------

# 4. Architectural Rules

## 4.1 Layering

The following dependency relationships apply.

Allowed:

```
runtime            → foundation
runtime            → platform
runtime            → core

platform           → core

platform_*         → platform
platform_*         → core
```

Not allowed:

```
foundation         → runtime
platform           → runtime
platform           → platform_*
core               → any upper layer
```

These rules preserve strict downward dependency flow.

------

## 4.2 Platform Contract Rules

### platform

This namespace defines portable system-effect contracts.

Examples include:

- clock services
- console services
- environment services
- filesystem services
- process services
- threading services
- terminal services
- system information

Platform contracts may include:

- interfaces (`*_service`, `*_provider`)
- small value types
- option or configuration structures
- enumerations
- portable error categories

Platform contracts must not:

- include OS headers
- perform syscalls
- expose OS-specific types
- perform OS error translation

------

### platform_*

Platform implementation namespaces provide OS-specific behavior.

Examples include:

```
platform_windows
platform_posix
```

These modules contain:

- OS API calls
- system calls
- handle or descriptor management
- OS error translation
- OS-specific helper logic

Platform implementations must not leak OS-specific types into portable contracts.

------

# 5. Canonical Runtime Capability Structure

Within:

```
nisanijo::mythos::daedalus::runtime
```

the following capability segmentation is typical.

```
app/
framework/
pipeline/
resource_manager/
state_machine/

platform/
    clock/
    console/
    environment/
    filesystem/
    process/
    registry/
    system_info/
    terminal/
    threading/
    user/

platform_windows/
    clock/
    console/
    environment/
    filesystem/
    process/
    registry/
    system_info/
    terminal/
    threading/
    user/
    debug/

platform_posix/
    clock/
    console/
    environment/
    filesystem/
    process/
    system_info/
    terminal/
    threading/
    user/
    debug/
```

Not all systems must implement every capability.

This taxonomy defines segmentation when such capabilities exist.

------

# 6. Public Interface Mapping

Public headers may expose runtime capabilities under:

```
include/nisanijo/mythos/daedalus/runtime/
```

Public interfaces must respect:

- layering constraints
- namespace-directory alignment
- platform boundary rules

Internal implementation details may remain private to the source tree.

------

# 7. Design Principles

Runtime and platform components must follow these principles.

### Separation of Concerns

Runtime orchestrates execution.

Foundation provides reusable infrastructure primitives.

Platform defines system-effect contracts.

Platform implementations translate OS semantics.

------

### Portable Contracts

Portable contracts must remain stable and platform-neutral.

Platform interfaces must not expose OS-specific constructs.

------

### OS Boundary Containment

Operating system APIs must appear only within platform implementation namespaces.

Portable layers must remain free of OS headers.

------

### Architectural Stability

Capability placement should prioritize long-term architectural clarity over convenience refactoring.

------

# 8. Enforcement

Runtime and platform taxonomy rules are enforced through architectural review.

Violations include:

- platform boundary violations
- incorrect layer placement
- leakage of OS-specific types into portable contracts
- introduction of upward dependency edges

CI validation may be introduced to detect deterministic violations such as:

- OS header inclusion within platform contracts
- forbidden dependency edges
- include-graph violations

------

# 9. References

```
architecture_layering
core_admission_and_elevation_policy
taxonomy_core_foundation
sdlc_framework_overview
taxonomy_glossary
```

