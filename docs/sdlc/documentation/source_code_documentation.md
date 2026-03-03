# Source Code Documentation Standard

## 1. Purpose

This document defines the minimum requirements for documenting source code
within SDLC-governed repositories.

Source documentation serves three primary purposes:

- Define the public contract of the system
- Communicate usage expectations and constraints
- Enable automated API reference generation

Documentation is part of the codebase and must evolve with it.


---

## 2. Scope

This standard applies to:

- Public headers
- Public classes
- Public functions
- Public enums
- Public templates
- Public macros (if exposed)
- Exported C interfaces

This standard does NOT require extensive documentation of:

- Private implementation details
- Internal helper functions
- Anonymous namespaces
- Test-only code

Over-documentation of private code is discouraged.


---

## 3. Public API Documentation Requirements

Every publicly exposed type or function MUST include:

### 3.1 Summary

A one-sentence description explaining what the entity does.

The summary must describe behavior, not implementation.


### 3.2 Contract Requirements

Where applicable, documentation MUST define:

- Preconditions
- Postconditions
- Ownership / lifetime expectations
- Thread-safety guarantees
- Error behavior

If a rule is implicit but critical, it must be made explicit.


---

## 4. Function Documentation Requirements

Public functions MUST document:

- Purpose
- Parameters (if non-trivial)
- Return value semantics
- Error conditions
- Side effects
- Thread-safety if relevant

Do not restate obvious type information.

Document semantics, not syntax.


---

## 5. Class Documentation Requirements

Public classes MUST document:

- High-level purpose
- Invariants
- Ownership model
- Thread-safety guarantees
- Usage expectations

If misuse is possible, document correct usage patterns.


---

## 6. Enum Documentation

Public enums MUST document:

- What the enum represents
- Meaning of each enumerator
- Whether new enumerators may be added in future versions

Enums that are part of ABI contracts must clearly indicate stability guarantees.


---

## 7. Templates and Generic Code

Templates must document:

- Required type constraints
- Expected semantics of template parameters
- Complexity guarantees if relevant

Do not rely solely on static assertions to communicate intent.


---

## 8. Error Model Documentation

Projects MUST clearly document one of the following error models:

- Exceptions
- Error codes / status objects
- `expected`-like return types
- Mixed model (if explicitly defined)

Each public API must align with the documented error model.

Error behavior must be consistent and predictable.


---

## 9. Thread-Safety Documentation

If a component:

- Is thread-safe
- Is conditionally thread-safe
- Is not thread-safe

This must be explicitly documented.

Absence of documentation implies NOT thread-safe.


---

## 10. Documentation Format

Projects SHOULD use a structured documentation style compatible with
automated tooling (e.g., Doxygen-style comments for C++).

Example style:

- Brief summary line
- Parameter descriptions
- Return description
- Contract notes

Formatting details are left to the project toolchain.

The SDLC does not mandate a specific documentation generator.


---

## 11. Prohibited Practices

The following are prohibited:

- Copying implementation code into documentation
- Documenting private implementation details unnecessarily
- Writing documentation that contradicts behavior
- Allowing stale documentation after behavior changes
- Using documentation as a substitute for clear API design


---

## 12. CI and Review Expectations

Projects SHOULD:

- Treat undocumented public APIs as review findings
- Optionally fail CI for undocumented public symbols
- Require documentation updates in PRs that change public behavior

Code review must consider documentation correctness as part of acceptance.


---

## 13. Minimal Compliance Checklist

A repository is compliant if:

- All public types and functions are documented
- Error model is documented
- Thread-safety is documented
- Public contracts are explicit
- Documentation is kept in sync with behavior

Failure to document public APIs constitutes SDLC non-compliance.
