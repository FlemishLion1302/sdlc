# API Reference Generation Standard

## 1. Purpose

This document defines the minimum requirements for generating and publishing API reference documentation in
SDLC-governed projects.

API reference documentation is a derived artifact:
- Generated from source code and source comments
- Reproducible in CI
- Tied to a specific commit/tag/version

Narrative documentation (docs/overview, docs/usage, docs/design, docs/dev) remains human-authored and is not replaced
by generated reference.


---

## 2. Scope

This standard applies to projects that expose a public API, including:

- C++ libraries (header-only, static, shared)
- C interfaces
- Tools with a stable CLI surface (optional, if treated as API)

Projects without a public surface may still generate internal reference, but are not required to publish it.


---

## 3. Tooling Baseline

Projects SHOULD use tooling compatible with the language and build system.

For C++ projects, Doxygen is the recommended baseline for reference generation.

The SDLC does not mandate a specific generator, but the chosen generator MUST:

- Support the project’s language
- Generate output deterministically in CI
- Emit non-zero exit status on failure
- Support linking symbols to source locations where practical


---

## 4. Output Location and Repository Structure

Implementation repositories MUST place generated API reference under:
```

docs/reference/

```
Recommended structure:
```

docs/reference/
├─ html/ (publishable output)
└─ xml/ (optional; for downstream tooling)

```
Rules:

- `docs/reference/` is generated output and MUST NOT be manually edited.
- Projects MAY keep generated reference out of the repository history (preferred) and publish via CI artifacts/pages.
- If generated reference is committed (discouraged), it must be clearly marked as generated and updated only via tooling.


---

## 5. Source Inclusion Rules

The API reference generator MUST be configured so that:

- Only the intended public surface is included by default
- Private/internal implementation details are excluded by default

Recommended conventions:

- Public headers live under a clear include root (e.g., `include/<project>/`)
- Internal headers live under internal roots (e.g., `src/<project>/detail/`, `internal/`, or equivalent)
- Reference output focuses on the public include root unless explicitly configured otherwise

If internal APIs are documented, they MUST be clearly labeled as internal and unstable.


---

## 6. Documentation Expectations for Public API

Generated API reference MUST be meaningful. For public APIs:

- Public types and functions MUST be documented (see `source_code_documentation.md`)
- Error behavior MUST be discoverable in the reference output
- Thread-safety and lifetime/ownership expectations MUST be discoverable in the reference output

If the generator supports warnings for undocumented symbols, projects SHOULD enable them.


---

## 7. CI Requirements

### 7.1 Generation

Projects MUST provide a reproducible CI step that:

- Builds (or configures) the documentation generator inputs
- Generates API reference output under `docs/reference/`
- Fails the build if generation fails

### 7.2 Artifact Handling

CI SHOULD:

- Upload generated HTML as a build artifact for pull requests
- Publish generated HTML for releases/tags

Publication targets may include:
- GitHub Pages
- Static hosting
- Release artifacts

### 7.3 Determinism

CI output SHOULD be stable across runs given identical inputs.

Projects SHOULD:
- Pin tool versions (container, package versions, or locked dependencies)
- Avoid embedding timestamps or machine-dependent paths where feasible


---

## 8. Release and Versioning Expectations

Published API reference MUST correspond to a specific version identifier.

On release/tag builds, the published reference SHOULD be associated with:
- semantic version tag, or
- release branch identifier, or
- commit hash if no versioning scheme exists

If multiple versions are published, the site should provide a stable path for:
- “latest” (optional; clearly defined)
- per-version snapshots


---

## 9. Configuration Governance

Projects MUST keep documentation generator configuration under version control.

For C++/Doxygen, this typically includes:
- a `Doxyfile` (or equivalent)
- optional custom header/footer/stylesheet assets

Configuration changes should be reviewed like code changes.

Projects SHOULD document:
- how to run generation locally
- where output lands
- how CI publishes it


---

## 10. Quality Gates (Pragmatic Defaults)

Projects SHOULD adopt these pragmatic gates:

- Generation failures fail CI.
- Warnings are visible in CI logs.
- “Undocumented public symbol” warnings are treated as review findings.
- Optionally: warnings-as-errors for public API on protected branches (recommended once the codebase is compliant).

The SDLC encourages incremental adoption:
- Start by generating reference reliably.
- Then increase strictness over time.


---

## 11. Minimal Compliance Checklist

A repository is compliant if:

- It can generate API reference in CI deterministically.
- Output is produced under `docs/reference/`.
- Generation failures fail CI.
- Public API documentation is present enough to make the output useful.
- Release builds publish or archive the generated reference.