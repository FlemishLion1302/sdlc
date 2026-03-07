# abi_policy

## Baseline Edition

**Status:** Authoritative Standard
**Companion To:** `cpp_coding_standard`
**Governed By:** `engineering_governance`

------

# 1. Purpose

This document defines how SDLC-governed projects manage ABI (Application Binary Interface) stability and how ABI compatibility is verified and reported.

ABI policy matters for projects that ship binary artifacts consumed by other binaries (for example shared libraries).

For projects that are header-only or do not publish binary interfaces, ABI requirements may be marked **Not Applicable**.

This policy is intentionally pragmatic:

- it defines a minimal policy surface
- it supports incremental adoption
- it requires clarity regarding what is and is not considered stable

------

# 2. Scope and Applicability

ABI policy applies when a project ships one or more of the following to external consumers:

- shared libraries (`.so`, `.dylib`, `.dll`)
- C ABI interfaces intended for dynamic linking
- plugins or modules loaded across version boundaries

ABI policy typically does **not** apply to:

- header-only libraries
- static libraries
- standalone executables (unless they provide plugin interfaces)

Header-only libraries still have **API stability concerns**, but ABI stability does not apply because consumers compile the code directly.

------

# 3. Definitions

### API

The source-level contract visible in headers.

Examples include:

- types
- functions
- templates
- macros

------

### ABI

The binary-level contract between compiled artifacts.

Examples include:

- exported symbols
- calling conventions
- type layout
- virtual tables
- alignment and padding
- exception handling behavior

------

### Public ABI Surface

The subset of compiled entities intended for external linking.

------

### ABI Break

A change that can break binary compatibility for a previously compiled consumer.

Examples include:

- removing exported symbols
- changing function signatures
- changing type layout
- altering virtual table ordering

------

# 4. Stability Levels

Projects that ship binaries **MUST** declare an ABI stability level for each published binary component.

This declaration **MUST** be visible to consumers through project documentation or release metadata.

Recommended stability levels are defined below.

------

## 4.1 Unstable ABI (Default)

Unstable ABI indicates that binary compatibility is not guaranteed.

Characteristics:

- ABI may change at any time
- consumers must rebuild when upgrading
- CI may produce ABI reports but does not gate merges

This level is appropriate for early development or internal distribution.

------

## 4.2 Stable ABI

Stable ABI indicates that binary compatibility is preserved within a declared compatibility line.

Characteristics:

- ABI is preserved across patch and minor releases
- ABI breaks require a major version bump or explicit compatibility boundary
- CI verification SHOULD gate compatibility changes

This level is appropriate when external consumers depend on binary compatibility.

------

# 5. Declaring the ABI Surface

Projects that ship binaries **MUST** define the public ABI surface.

At minimum the following must be defined:

- which libraries constitute public ABI
- which headers define the public API surface
- whether the ABI promise applies to C++ symbols or only a C ABI wrapper

Pragmatic recommendation:

Projects SHOULD prefer a **C ABI wrapper** for long-lived stable interfaces.

Stable C++ ABI is expensive to maintain and sensitive to compiler and toolchain variation.

------

# 6. Versioning Rules

If a component is declared **Stable ABI**, the project **MUST** define how versioning relates to compatibility.

Typical expectations:

- patch releases — no ABI breaks
- minor releases — no ABI breaks within the same major version
- major releases — ABI breaks allowed

If the project uses a different compatibility scheme, it **MUST** be documented.

------

# 7. ABI Verification Tooling

Projects **SHOULD** use automated tooling to detect ABI changes.

Common approaches include:

- `libabigail` (`abidiff`) for ELF platforms
- ABI Compliance Checker workflows
- platform-specific ABI comparison tools

The SDLC framework does not mandate a specific tool.

However the toolchain **MUST**:

- produce comparison reports
- compare current builds against a compatibility baseline
- run deterministically in CI

Tool versions **SHOULD** be pinned to ensure reproducible results.

------

# 8. Baselines

Projects with ABI verification **MUST** define baseline management.

Recommended strategy:

- baselines are generated from release tags
- the baseline corresponds to the most recent compatible release
- CI compares pull requests against the baseline

Baseline storage options include:

- CI artifacts produced during releases
- release assets in a separate repository
- a dedicated baselines repository

Storing baselines directly in the source repository is discouraged unless clearly marked as generated artifacts.

------

# 9. CI Expectations

## 9.1 Report-Only Adoption

Projects adopting ABI verification SHOULD initially run checks in report-only mode.

In this mode:

- CI produces ABI comparison reports
- reports are uploaded as CI artifacts
- ABI differences do not fail the build

This stage helps establish baselines and eliminate noise.

------

## 9.2 Gated Adoption

For components declared **Stable ABI**, CI SHOULD gate compatibility.

Typical behavior:

- incompatible ABI change → CI failure
- compatible change → allowed

Intentional ABI breaks **MUST** be accompanied by:

- version changes consistent with policy
- release notes describing the change

------

# 10. Build Configuration for ABI Analysis

ABI analysis typically requires:

- a consistent compiler toolchain
- debug information
- stable build flags

Projects SHOULD provide a dedicated configuration for ABI analysis.

For example:

```
release + debuginfo
```

Projects claiming stable C++ ABI SHOULD avoid mixing toolchains across compatibility lines.

------

# 11. Documentation and Consumer Guidance

If ABI stability is declared, projects **MUST** document:

- supported platforms and toolchains
- compatibility guarantees
- upgrade expectations for consumers

If ABI is unstable, projects **SHOULD** clearly state:

```
Consumers must rebuild when upgrading.
```

------

# 12. Minimal Compliance Checklist

A project is considered ABI-compliant if:

- it declares an ABI stability level for each shipped binary
- it defines the public ABI surface
- CI produces ABI comparison reports
- stable ABI components have a baseline strategy
- release processes generate baselines for compatibility lines

