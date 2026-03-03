# ABI Policy Standard

## 1. Purpose

This document defines how SDLC-governed projects manage ABI (Application Binary Interface) stability and how ABI
compatibility is verified and reported.

ABI policy matters for projects that ship binary artifacts consumed by other binaries (e.g., shared libraries).
For projects that are header-only or do not publish binary interfaces, ABI requirements may be marked N/A.

This standard is pragmatic:
- It defines a minimum policy surface
- It enables incremental adoption (report-only → gated)
- It requires clarity about what is and is not stable


---

## 2. Scope and Applicability

ABI policy applies when a project ships one or more of the following to external consumers:

- Shared libraries (`.so`, `.dylib`, `.dll`)
- C ABI interfaces intended for dynamic linking
- Plugins/modules loaded by a host process across version boundaries

ABI policy is typically NOT applicable for:
- Header-only libraries (API stability applies; ABI is consumer-compiled)
- Static libraries (ABI is less meaningful across independently built consumers)
- Standalone executables (unless providing plugin ABI)


---

## 3. Definitions

- **API**: Source-level contract visible in headers (types, functions, templates, macros).
- **ABI**: Binary-level contract (symbols, calling conventions, layout, vtables, alignment, etc.).
- **Public ABI surface**: The subset of compiled entities intended for external linking.
- **ABI break**: A change that can break binary compatibility for an already-built consumer.


---

## 4. Stability Levels

Each project that ships binaries MUST declare an ABI stability level for each published binary component.

Recommended levels:

### 4.1 Unstable ABI (default)

- ABI may change at any time.
- Consumers must rebuild when upgrading.
- CI may produce ABI reports, but does not gate merges.

Use for early development or internal-only distribution.


### 4.2 Stable ABI (versioned)

- ABI is preserved across patch/minor releases within a declared compatibility line.
- ABI breaks require a major version bump or explicitly declared compatibility boundary.

Use when external consumers depend on binary compatibility.


---

## 5. Declaring the ABI Surface

Projects that ship binaries MUST define the public ABI surface.

At minimum, define:
- Which libraries are considered public ABI (names/artifacts)
- Which headers define the public API for those libraries
- Whether C++ ABI is part of the promise, or only a C ABI wrapper is stable

Pragmatic recommendation:
- Prefer a **C ABI wrapper** for long-lived stable binary interfaces.
- Treat “stable C++ ABI” as high-cost and toolchain-sensitive unless tightly controlled.


---

## 6. Versioning Rules

If a component is declared **Stable ABI**, the project MUST define how versioning relates to compatibility.

Pragmatic default expectations:
- Patch releases: no ABI breaks
- Minor releases: no ABI breaks (within the same major)
- Major releases: ABI breaks allowed

If the project uses a different scheme, it must be stated explicitly.


---

## 7. ABI Verification Tooling

Projects SHOULD use an automated ABI comparison tool.

Common approaches include:
- libabigail (`abidiff`) for ELF-based platforms
- ABI Compliance Checker / abi-dumper workflows
- Platform-specific equivalents as needed

The SDLC does not mandate a specific tool, but the toolchain MUST:
- Produce a report artifact suitable for review
- Compare “current build” vs “baseline” for a compatibility line
- Run in CI deterministically (pinned tool versions recommended)


---

## 8. Baselines

Projects with ABI checks MUST define where baselines live and how they are updated.

Pragmatic baseline strategy:

- A baseline is generated on release tags.
- The baseline corresponds to the last released version in a compatibility line.
- CI compares PR builds against the most recent baseline.

Baseline storage options:
- Stored as CI artifacts in the release pipeline (preferred)
- Stored in a separate “baselines” repository or release assets (preferred)
- Stored in-repo (discouraged unless unavoidable; must be marked generated)


---

## 9. CI Expectations

### 9.1 Report-Only Adoption (recommended first step)

- CI generates ABI comparison reports for PRs
- CI uploads reports as artifacts
- ABI differences do not fail the build

This is suitable while establishing a baseline and cleaning up noise.


### 9.2 Gated Adoption (for Stable ABI components)

For components declared **Stable ABI**, protected branches SHOULD gate on ABI compatibility:

- ABI break → CI failure unless explicitly approved by policy (e.g., on major bump branch)
- ABI changes that are compatible may pass (tool-dependent)

Exception handling:
- If an ABI break is intentional, it MUST be accompanied by:
  - versioning action consistent with policy (e.g., major bump), and
  - release notes / changelog entry


---

## 10. Build Configuration for ABI Analysis

ABI tooling typically requires:
- consistent compiler/toolchain
- debug info (recommended)
- stable build flags

Projects SHOULD define a dedicated build configuration for ABI analysis (e.g., “release+debuginfo”)
to reduce false positives and improve report quality.

Projects SHOULD avoid mixing toolchains when claiming stable C++ ABI.


---

## 11. Documentation and Consumer Guidance

If ABI is declared stable, projects MUST document:

- supported platforms and toolchains for ABI compatibility
- whether consumers may mix compilers/standard libraries
- the upgrade path and compatibility line definitions

If ABI is declared unstable, projects SHOULD state:
- “Consumers must rebuild when upgrading.”


---

## 12. Minimal Compliance Checklist

A project is ABI-compliant if:

- It declares whether ABI stability is Unstable or Stable for each shipped binary.
- It defines the ABI surface (which libraries/interfaces are public ABI).
- It generates ABI comparison reports in CI (at least report-only).
- Stable ABI components have a baseline strategy and a path to gating.
- Releases include baseline generation (for stable ABI components).