# deprecation_policy

## 1. Purpose

This document defines how SDLC-governed projects deprecate and remove public APIs.

Deprecation provides a controlled migration path for consumers while maintaining stability guarantees appropriate to the project’s declared API and ABI policies.

Deprecation ensures that public surface changes occur in a predictable and documented manner.

------

## 2. Scope

This policy applies to all SDLC-governed projects that expose public APIs.

The policy governs the deprecation lifecycle for:

- public classes
- public functions
- public enums and enumerators
- public typedefs and aliases
- exported C interfaces
- public macros

This policy does **not** apply to:

- private or internal code
- APIs explicitly declared experimental or unstable

------

## 3. Authority

This document forms part of the SDLC governance framework.

Its authority derives from:

- `sdlc_framework_overview`
- `sdlc_governance`

This policy works in conjunction with:

- `versioning_policy`
- `abi_policy` (where applicable)

Engineering standards may introduce additional constraints but may not weaken the requirements defined here.

------

## 4. Normative Language

Normative language follows the definitions established in the SDLC document standard.

The keywords **MUST**, **MUST NOT**, **SHOULD**, and **MAY** have the meanings defined in RFC-2119.

| Keyword  | Meaning               |
| -------- | --------------------- |
| MUST     | Mandatory requirement |
| MUST NOT | Prohibited            |
| SHOULD   | Strong recommendation |
| MAY      | Optional behavior     |

------

## 5. Stability Prerequisite

Before applying this policy, projects **MUST** declare:

- whether the public API is considered stable
- whether the ABI is considered stable (if applicable)

If the API is declared unstable:

- formal deprecation **MAY** be skipped
- removal **MUST** still be documented in `CHANGELOG.md`

------

## 6. Deprecation Lifecycle

The standard lifecycle for a stable API is:

### 6.1 Phase 1 — Active

- Fully supported.
- No deprecation warnings.

------

### 6.2 Phase 2 — Deprecated

The API remains functional but is scheduled for removal.

Requirements:

- source annotation indicating deprecation
- documentation marking the API deprecated
- documented migration path
- optional compile-time warnings

------

### 6.3 Phase 3 — Removed

The API is removed from the public surface.

Requirements:

- removal documented in `CHANGELOG.md`
- version increment consistent with `versioning_policy`

------

## 7. Minimum Deprecation Requirements

When deprecating a public API, the following are required:

1. Source annotation (e.g., `[[deprecated]]` in C++ where applicable)
2. Documentation update indicating:
   - the API is deprecated
   - the version in which deprecation occurred
   - the recommended replacement
3. `CHANGELOG.md` entry under “Deprecated”
4. documented migration path

Deprecation without migration guidance **MUST NOT** occur.

------

## 8. Deprecation Duration

Projects **MUST** define a minimum supported deprecation window.

Recommended default policy:

- PATCH releases: no removals
- MINOR releases: deprecations allowed
- MAJOR releases: removals allowed

Alternative policies **MAY** be adopted but **MUST** be documented.

------

## 9. ABI Considerations

For components with **Stable ABI**:

- removing or altering deprecated symbols within a compatible release line is prohibited
- deprecated symbols **MUST** remain ABI-compatible until the compatibility boundary ends
- removal **MUST** occur at a version boundary consistent with `abi_policy`

If ABI is declared unstable:

- removal **MAY** occur without ABI guarantees
- documentation and versioning rules still apply

------

## 10. Documentation Requirements

Reference documentation **MUST**:

- clearly mark deprecated entities
- display deprecation notices prominently
- preserve documentation of deprecated APIs until removal

Deprecated APIs **MUST NOT** disappear from documentation without a documented version boundary.

------

## 11. Prohibited Practices

The following practices are prohibited:

- silent removal of public APIs
- deprecation without migration guidance
- incompatible behavioral changes to deprecated APIs
- using deprecation as a substitute for proper versioning discipline

------

## 12. CI and Review Expectations

Projects **SHOULD**:

- enable compiler warnings for deprecated usage internally
- track usage of deprecated APIs within the repository
- prevent new internal dependencies on deprecated APIs

Code review **MUST** treat removal without prior deprecation as a policy violation unless the API is explicitly unstable.

------

## 13. Minimal Compliance Checklist

A repository is compliant with this policy if:

- public APIs are not removed without prior deprecation (for stable APIs)
- deprecated APIs are annotated in source
- migration paths are documented
- `CHANGELOG.md` reflects deprecations and removals
- versioning aligns with removal events

------

## 14. Amendment and Evolution

Changes to this document follow the SDLC amendment model.

Amendments **SHOULD** normally be introduced through amendment documents rather than direct modification of the baseline.

Amendment mechanics are defined in:

- `sdlc_governance`
- `sdlc_document_standard`

Over time, amendments may be consolidated into revised baseline editions.

------

## 15. References

- `sdlc_framework_overview`
- `sdlc_governance`
- `sdlc_document_standard`
- `servicing_and_maintenance_strategy`
- `versioning_policy`
- `abi_policy`

