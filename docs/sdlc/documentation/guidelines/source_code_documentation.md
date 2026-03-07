# source_code_documentation

## 1. Purpose

This document defines the minimum requirements for documenting source code within SDLC-governed repositories.

Source documentation serves three primary purposes:

- define the public contract of the system
- communicate usage expectations and constraints
- enable automated API reference generation

Documentation embedded in source code is considered part of the implementation and must evolve with the code it describes.

------

## 2. Scope

This standard governs documentation embedded within source code.

It applies to:

- public headers
- public classes
- public functions
- public enums
- public templates
- public macros (if exposed)
- exported C interfaces

This standard does **not** require extensive documentation of:

- private implementation details
- internal helper functions
- anonymous namespaces
- test-only code

Over-documentation of private code is discouraged.

Repository-level documentation requirements are defined in:

- `project_documentation`

API reference generation is governed by:

- `api_reference_generation`

------

## 3. Authority

This document forms part of the SDLC documentation governance domain.

Its authority derives from:

- `sdlc_framework_overview`
- `sdlc_governance`
- `documentation_governance`

Engineering standards may impose stricter documentation requirements but may not weaken the requirements defined here.

------

## 4. Public API Documentation Requirements

Every publicly exposed type or function **MUST** include documentation.

Public APIs include:

- exported classes
- public methods
- public functions
- externally consumable interfaces
- public templates
- exported enumerations

Documentation must describe **behavior and contract**, not implementation details.

------

## 5. Documentation Elements

Public APIs **MUST** include the following information where applicable.

### 5.1 Summary

A concise description explaining what the entity does.

The summary must describe behavior rather than implementation.

------

### 5.2 Contract Requirements

Documentation **MUST** define important behavioral constraints including:

- preconditions
- postconditions
- ownership or lifetime expectations
- thread-safety guarantees
- error behavior

Implicit but critical rules must be made explicit.

------

## 6. Function Documentation Requirements

Public functions **MUST** document:

- purpose
- parameters (when semantics are non-trivial)
- return value semantics
- error conditions
- side effects
- thread-safety guarantees if applicable

Documentation should describe semantics rather than restating obvious type information.

------

## 7. Class Documentation Requirements

Public classes **MUST** document:

- high-level purpose
- invariants
- ownership model
- thread-safety guarantees
- expected usage patterns

If misuse is possible, documentation should describe correct usage.

------

## 8. Enum Documentation

Public enums **MUST** document:

- what the enumeration represents
- meaning of each enumerator
- whether additional enumerators may appear in future versions

Enums forming part of ABI contracts must clearly indicate stability expectations.

------

## 9. Templates and Generic Code

Templates must document:

- required type constraints
- expected semantics of template parameters
- complexity guarantees when relevant

Static assertions should not be the sole mechanism used to communicate constraints.

------

## 10. Error Model Documentation

Projects **MUST** clearly document their error model.

Typical models include:

- exceptions
- error codes or status objects
- `expected`-style return types
- a mixed model when explicitly defined

Public APIs must behave consistently with the documented error model.

------

## 11. Thread-Safety Documentation

If a component:

- is thread-safe
- is conditionally thread-safe
- is not thread-safe

this must be explicitly documented.

Absence of documentation implies the component is **not thread-safe**.

------

## 12. Documentation Format

Projects **SHOULD** use a structured documentation format compatible with automated tooling.

Typical examples include Doxygen-style documentation comments.

Common structure includes:

- summary line
- parameter descriptions
- return value description
- contract notes

The SDLC does not mandate a specific documentation generator.

------

## 13. Prohibited Practices

The following practices are prohibited:

- copying implementation code into documentation
- documenting private implementation details unnecessarily
- writing documentation that contradicts behavior
- allowing stale documentation after behavior changes
- using documentation as a substitute for clear API design

------

## 14. CI and Review Expectations

Projects **SHOULD** treat undocumented public APIs as review findings.

Projects may optionally:

- fail CI for undocumented public symbols
- enforce documentation coverage for public APIs
- require documentation updates when public behavior changes

Documentation correctness must be evaluated during code review.

------

## 15. Minimal Compliance Checklist

A repository is compliant with this specification if:

- public APIs are documented
- the error model is documented
- thread-safety guarantees are documented
- behavioral contracts are explicit
- documentation remains synchronized with implementation behavior

Failure to document public APIs constitutes SDLC non-compliance.

------

## 16. Amendment and Evolution

Changes to this document follow the SDLC amendment model.

Amendment mechanics are defined in:

- `sdlc_governance`
- `sdlc_document_standard`

Amendments may later be consolidated into revised baseline documents.

------

## 17. References

- `sdlc_framework_overview`
- `sdlc_governance`
- `sdlc_document_standard`
- `documentation_governance`
- `project_documentation`
- `api_reference_generation`

