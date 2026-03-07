# versioning_policy

Document Identifier: SDLC-DOC-006

## 1. Purpose

This document defines how SDLC-governed projects version their releases and how version numbers communicate compatibility expectations.

Version numbers serve as contractual signals to consumers and indicate the scope of changes between releases.

This policy defines how version numbers relate to:

- API stability
- ABI stability (if applicable)
- Deprecation and removal
- Compatibility expectations

------

## 2. Scope

This policy applies to all SDLC-governed projects that publish versioned releases.

It governs:

- version numbering schemes
- compatibility expectations associated with version components
- alignment between versioning, API stability, and ABI stability
- release tagging practices

Language-specific rules or packaging details may impose additional constraints but must not weaken the requirements defined here.

------

## 3. Authority

This document forms part of the SDLC governance framework.

Its authority derives from:

- `sdlc_framework_overview`
- `sdlc_governance`

Documentation governance may reference this policy when defining compatibility guarantees.

Engineering standards may impose additional requirements but may not weaken the rules defined here.

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

## 5. Versioning Scheme

Projects **MUST** declare their versioning scheme.

The recommended default is:

```
MAJOR.MINOR.PATCH
```

Projects **MAY** adopt an alternative scheme, but the compatibility meaning of each component **MUST** be clearly defined.

Unless otherwise documented, this policy assumes:

- **MAJOR** → incompatible changes allowed
- **MINOR** → backward-compatible additions only
- **PATCH** → backward-compatible fixes only

------

## 6. Semantic Meaning (Default Policy)

### 6.1 PATCH

Permitted:

- bug fixes
- documentation updates
- internal refactoring
- performance improvements

Not permitted:

- API removal
- API behavior changes
- ABI breaks (if ABI is declared stable)

------

### 6.2 MINOR

Permitted:

- new API additions
- backward-compatible extensions
- deprecations (with migration path)
- non-breaking behavior refinements

Not permitted:

- API removals (for stable APIs)
- ABI breaks within the same MAJOR line (if ABI is declared stable)

------

### 6.3 MAJOR

Permitted:

- API removals
- breaking API changes
- ABI breaks
- redesigns

Major releases **MUST** clearly document breaking changes.

------

## 7. API Stability Expectations

Projects **MUST** declare whether their public API is:

- Stable
- Experimental
- Unstable

If API is declared **Stable**:

- backward compatibility **MUST** be preserved within the same MAJOR version
- removals **MUST** require a MAJOR increment

If API is declared **Unstable**:

- version numbers **MAY** reflect maturity but do not guarantee compatibility
- breaking changes **MAY** occur in MINOR releases if explicitly documented

------

## 8. ABI Stability Expectations

If a project ships binary artifacts and declares **Stable ABI**:

- ABI **MUST** remain compatible within the same MAJOR version
- ABI breaks **MUST** require a MAJOR increment
- ABI changes **SHOULD** be verified using automated tooling (see `abi_policy`)

If ABI is **Unstable**:

- consumers **MUST** rebuild on upgrade
- ABI changes **MAY** occur at any release boundary
- this condition **MUST** be clearly documented

------

## 9. Deprecation Alignment

Deprecation and versioning **MUST** align.

For **Stable API** projects:

- deprecations **MAY** occur in MINOR releases
- removal **MUST** occur only in MAJOR releases

For **Stable ABI** projects:

- deprecated symbols **MUST** remain ABI-compatible until the next MAJOR boundary

Removals without prior deprecation are prohibited unless the API is declared unstable.

------

## 10. Pre-1.0 Projects

Projects below version **1.0** **MAY** adopt a relaxed compatibility policy.

Recommended pragmatic approach:

```
0.MINOR.PATCH
```

- breaking changes allowed in MINOR increments
- PATCH reserved for fixes only

Once **1.0.0** is declared, the compatibility rules defined in this policy apply.

------

## 11. Release Tagging

Releases **MUST**:

- be tagged in version control
- correspond to a reproducible build
- include a CHANGELOG entry

Published documentation and ABI baselines (if applicable) **MUST** correspond to the release tag.

------

## 12. Build Metadata and Pre-Releases

Projects **MAY** use:

- pre-release identifiers (e.g. `-alpha`, `-beta`, `-rc`)
- build metadata identifiers

Pre-releases **SHOULD NOT** be treated as stability guarantees unless explicitly declared.

------

## 13. Prohibited Practices

The following practices are prohibited:

- changing public behavior without a version increment
- removing public API without a MAJOR increment for stable APIs
- introducing ABI breaks in PATCH or MINOR releases when ABI is declared stable
- re-tagging released versions with different content

------

## 14. Minimal Compliance Checklist

A repository is compliant with this policy if:

- a versioning scheme is declared
- compatibility meaning of version components is documented
- breaking changes align with version increments
- deprecation and removal follow the declared versioning rules
- releases are tagged and reproducible

------

## 15. Amendment and Evolution

Changes to this document follow the SDLC amendment model.

Amendments **SHOULD** normally be introduced through amendment documents rather than modifying the baseline directly.

Amendment mechanics are defined in:

- `sdlc_governance`
- `sdlc_document_standard`

Over time, amendments may be consolidated into revised baseline editions.

------

## 16. References

- `sdlc_framework_overview`
- `sdlc_governance`
- `sdlc_document_standard`
- `servicing_and_maintenance_strategy`

