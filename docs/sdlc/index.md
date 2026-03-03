# Documentation Governance

This directory defines documentation and compatibility governance
requirements for SDLC-governed projects.

Documentation is treated as a first-class artifact:
- It defines system contracts.
- It communicates stability guarantees.
- It enables automated reference generation.
- It evolves alongside source code.

This section defines minimum compliance requirements for documentation,
API reference generation, ABI stability, deprecation, and versioning.


---

# Document Map

## 1. Project Documentation

**project_documentation.md**

Defines:
- Required repository-level documents (README, CHANGELOG, LICENSE)
- Required `docs/` structure in implementation repositories
- Documentation categories (overview, usage, design, dev)
- CI expectations for documentation publishing


---

## 2. Source Code Documentation

**source_code_documentation.md**

Defines:
- What must be documented in public APIs
- Documentation contract expectations
- Error model and thread-safety requirements
- CI and review expectations for public documentation


---

## 3. API Reference Generation

**api_reference_generation.md**

Defines:
- Requirements for generating API reference documentation
- Output location (`docs/reference/`)
- CI reproducibility expectations
- Public surface inclusion rules


---

## 4. ABI Policy

**abi_policy.md**

Defines:
- When ABI stability applies
- Stable vs Unstable ABI levels
- Baseline management
- CI verification expectations


---

## 5. Deprecation Policy

**deprecation_policy.md**

Defines:
- Required lifecycle for deprecating APIs
- Migration path requirements
- Alignment with versioning
- ABI considerations for deprecated symbols


---

## 6. Versioning Policy

**versioning_policy.md**

Defines:
- Versioning scheme expectations
- Compatibility guarantees by version component
- Alignment with API and ABI stability
- Release tagging requirements


---

# Governance Principles

All SDLC-governed projects must:

- Treat documentation as part of the product.
- Keep documentation version-controlled.
- Keep documentation aligned with behavior.
- Generate reference documentation deterministically.
- Align versioning, deprecation, and ABI guarantees.

Projects may exceed these standards.

They may not fall below them.


---

# Relationship to Other SDLC Sections

- Language-specific rules (e.g., C++ coding standards) define how APIs are written.
- Documentation governance defines how APIs are described and published.
- Tooling governance defines how automation is structured and executed.

Together, these form the complete documentation lifecycle:
design → implementation → documentation → generation → publication → evolution.