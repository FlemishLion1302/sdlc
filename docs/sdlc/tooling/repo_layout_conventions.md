# Repository Layout Conventions

## 1. Purpose

This document defines pragmatic repository layout conventions for SDLC-governed projects.

The goal is structural consistency across repositories without over-constraining
implementation details.

Consistency improves:
- Navigability
- Onboarding speed
- Automation reliability
- Cross-project tooling reuse


---

## 2. Guiding Principles

Repositories should be:

- Predictable in structure
- Clear in separation of responsibilities
- Deterministic in dependency direction
- Explicit about public vs internal surface

Layout must support maintainability over time.


---

## 3. Recommended Top-Level Layout

Implementation repositories SHOULD follow:
```

/
├─ include/ (public headers, if applicable)
├─ src/ (implementation sources)
├─ tests/ (unit/integration tests)
├─ tools/ (automation and developer tooling)
├─ docs/ (documentation content and generated artifacts)
├─ third_party/ (vendored dependencies, if used)
├─ CMakeLists.txt (or equivalent build root)
└─ .github/ (CI workflows, if applicable)

```
Projects may add additional directories as needed,
but deviation from this structure should be justified.


---

## 4. Public vs Internal Code Separation

Projects that expose a public API MUST separate:

- Public headers → `include/`
- Implementation sources → `src/`
- Internal headers → `src/**/detail/` or equivalent

Rules:

- Public headers must not depend on internal headers.
- Internal headers must not leak into the public include surface.
- Dependency direction must flow inward, not outward.


---

## 5. Documentation Layout (Implementation Repositories)

Projects generating documentation MUST use:
```

docs/
├─ index.md
├─ overview/
├─ usage/
├─ design/
├─ dev/
├─ reference/ (generated)
└─ abi/ (generated, if applicable)

```
Rules:

- `reference/` contains generated API documentation only.
- `abi/` contains generated ABI baselines and reports only.
- Human-authored documentation must not be mixed with generated artifacts.


---

## 6. Tooling Layout

Automation MUST reside under:
```

tools/

```
Recommended structure:
```

tools/
├─ docs/
├─ abi/
├─ build/
├─ test/
├─ lint/
└─ release/

```
Rules:

- Do not place automation scripts in `src/`.
- Do not place automation scripts in `docs/`.
- Tooling must be discoverable and logically grouped.


---

## 7. Tests

Tests SHOULD reside under:
```

tests/

```
Rules:

- Tests must not live inside `src/`.
- Tests must not expose private internals as part of the public API.
- Test-only helpers should remain test-local.


---

## 8. Generated Artifacts

Generated artifacts must not pollute source directories.

Generated output should reside in:

- `docs/reference/`
- `docs/abi/`
- Build output directories outside source control (preferred)

Committing generated artifacts is discouraged unless required by distribution needs.


---

## 9. Dependency Direction

Repositories SHOULD enforce:

- Public interface stability
- No cyclic dependencies between modules
- Clear inward dependency flow (API → implementation → detail)

Structural determinism takes precedence over convenience.


---

## 10. Minimal Compliance Checklist

A repository is compliant if:

- It follows a stable and predictable top-level layout.
- Public and internal code are separated.
- Tooling is separated from runtime code.
- Generated documentation uses `docs/reference/`.
- ABI reports (if applicable) use `docs/abi/`.
- Tests are isolated under `tests/`.
