# taxonomy_core_foundation

Status: Normative Architecture Taxonomy
Domain: architecture
Scope: Capability segmentation within the `daedalus::foundation` layer

------

# 1. Purpose

This document defines the capability taxonomy for the foundation layer.

The foundation layer provides platform-agnostic infrastructure that builds on the core and platform layers.

It contains reusable building blocks including:

- diagnostics
- configuration systems
- data structures
- text processing
- serialization mechanisms
- cryptographic primitives
- compile-time utilities
- optional design pattern implementations

This taxonomy defines **how these capabilities are segmented** within the foundation layer.

It does not define coding style or repository layout rules.

Coding conventions are governed by the C++ Coding Standard.

Source-tree mapping is defined in:

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

Terminology used in this document is defined in:

```
sdlc_glossary
```

------

# 3. Foundation Layer Context

The foundation layer resides within:

```
nisanijo::mythos::daedalus::foundation
```

It is positioned above:

```
daedalus::core
daedalus::platform
```

Foundation provides reusable infrastructure and policy mechanisms.

Foundation must not depend on:

```
daedalus::runtime
```

Higher layers may depend on foundation.

------

# 4. Architectural Rules

## 4.1 Layering

Within the infrastructure layer hierarchy:

```
runtime → foundation → platform → core
```

Foundation must not introduce dependencies on runtime.

------

## 4.2 Responsibility Boundaries

### Diagnostics vs Logging

Diagnostics defines the diagnostic API surface.

Logger sinks implement output mechanisms.

Allowed relationship:

```
diagnostics → io::sinks::logger
```

Logger implementations must not depend on diagnostics.

------

### Console vs UI

Low-level device interaction belongs to:

```
io::console
```

User interaction models belong to:

```
ui
```

If the component models a device interface it belongs in `io`.

If it models user interaction or workflows it belongs in `ui`.

------

### Compile-Time vs Runtime

Compile-time utilities reside in:

```
meta
```

The `meta` namespace must contain only compile-time constructs and must not introduce runtime behavior.

------

# 5. Canonical Foundation Capability Structure

Within the foundation namespace:

```
nisanijo::mythos::daedalus::foundation
```

the following capability segmentation is canonical.

```
archive/

config/
    cli/
        cmd_args/
    env/
        env_vars/
    settings/
        core/
        app_specific/
    validation/

core/
    byte/
    err/
    mem/
        byte/
        buffer/
        bitset/
    time/
        date/
        time/
        datetime/
        calendar/
        duration/
        timezone/
        archive/

crypto/
    hash/
    digest/
        sha256/
            spec/

data/
    canonical/
    color/
    counter/
    dictionary/
    keyval/
    sparse_table/
    tree/
        red_black_tree/

diagnostics/
    core/
        assert/
        diag/
        error/
        exception/
        source_location/

io/
    console/
    file/
    media/
    serialization/
    sinks/
        logger/
        terminal/

math/
    stats/

meta/
    canonical/
    interfaces/
    type_traits/

patterns/
    facades/
    mvc/
    observer/
    producer_consumer/

text/
    char/
    encoding/
    lex/
    parser/
    regex/
    string/
    template_engine/

ui/
    color/
    controls/
    framework/
    mvc/
    ux_utils/
```

Not all capabilities are required in every system.

This taxonomy defines segmentation when those capabilities exist.

------

# 6. Public Interface Mapping

Public headers may expose foundation capabilities under:

```
include/nisanijo/mythos/daedalus/foundation/
```

The public API surface should mirror the capability segmentation where appropriate.

However, not all internal capabilities must be exposed publicly.

Internal implementation details may remain private to the source tree.

------

# 7. Design Principles

Foundation components must satisfy the following principles.

### Platform Independence

Foundation must remain platform-agnostic.

System effects must be mediated through platform contracts.

------

### Reusability

Foundation components must be reusable across multiple layers and subsystems.

------

### Separation of Concerns

Clear separation must be maintained between:

- API surfaces and backend implementations
- user interaction and device interaction
- compile-time and runtime constructs

------

### Architectural Stability

Capability placement should favor long-term architectural clarity over short-term convenience refactoring.

------

# 8. Enforcement

Foundation taxonomy rules are enforced through architectural review.

Violations include:

- capability misplacement
- foundation/runtime dependency violations
- boundary violations between capability groups

CI validation may be introduced in the future to detect deterministic violations.

------

# 9. References

```
architecture_layering
core_admission_and_elevation_policy
sdlc_framework_overview
taxonomy_glossary
```

