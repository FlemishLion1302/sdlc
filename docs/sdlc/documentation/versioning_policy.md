# Versioning Policy Standard

## 1. Purpose

This document defines how SDLC-governed projects version their releases and
how version numbers relate to:

- API stability
- ABI stability (if applicable)
- Deprecation and removal
- Compatibility expectations

Version numbers are contractual signals to consumers.


---

## 2. Versioning Scheme

Projects MUST declare their versioning scheme.

The recommended default is:

    MAJOR.MINOR.PATCH

Projects may adopt an alternative scheme, but the compatibility meaning of
each component MUST be clearly defined.

Unless otherwise documented, this standard assumes:

- MAJOR → incompatible changes allowed
- MINOR → backward-compatible additions only
- PATCH → backward-compatible fixes only


---

## 3. Semantic Meaning (Default Policy)

Under the default MAJOR.MINOR.PATCH scheme:

### 3.1 PATCH

Permitted:
- Bug fixes
- Documentation updates
- Internal refactoring
- Performance improvements

Not permitted:
- API removal
- API behavior changes
- ABI breaks (if ABI is declared stable)


### 3.2 MINOR

Permitted:
- New API additions
- Backward-compatible extensions
- Deprecations (with migration path)
- Non-breaking behavior refinements

Not permitted:
- API removals (for stable APIs)
- ABI breaks within the same MAJOR line (if ABI is declared stable)


### 3.3 MAJOR

Permitted:
- API removals
- Breaking API changes
- ABI breaks
- Redesigns

Major releases must document breaking changes clearly.


---

## 4. API Stability Expectations

Projects MUST declare whether their public API is:

- Stable
- Experimental
- Unstable

If API is declared Stable:

- Backward compatibility must be preserved within the same MAJOR version.
- Removals require MAJOR increment.

If API is declared Unstable:

- Version numbers may reflect maturity but do not guarantee compatibility.
- Breaking changes may occur in MINOR releases if explicitly documented.


---

## 5. ABI Stability Expectations

If a project ships binary artifacts and declares Stable ABI:

- ABI must remain compatible within the same MAJOR version.
- ABI breaks require MAJOR increment.
- ABI changes must be verified using automated tooling (see ABI Policy).

If ABI is Unstable:

- Consumers must rebuild on upgrade.
- ABI changes may occur at any release boundary.
- This must be documented clearly.


---

## 6. Deprecation Alignment

Deprecation and versioning MUST align.

For Stable API projects:

- Deprecations may occur in MINOR releases.
- Removal must occur only in MAJOR releases.

For Stable ABI projects:

- Deprecated symbols must remain ABI-compatible until MAJOR boundary.

Removals without prior deprecation are prohibited unless the API is declared unstable.


---

## 7. Pre-1.0 Projects

Projects below 1.0 MAY adopt a relaxed policy.

Recommended pragmatic approach:

- 0.MINOR.PATCH
- Breaking changes allowed in MINOR increments.
- PATCH reserved for fixes only.

However, once 1.0.0 is declared, full compatibility rules apply.


---

## 8. Release Tagging

Releases MUST:

- Be tagged in version control.
- Correspond to a reproducible build.
- Have a CHANGELOG entry.

Published documentation and ABI baselines (if applicable)
must correspond to the release tag.


---

## 9. Build Metadata and Pre-Releases

Projects MAY use:

- Pre-release identifiers (e.g., -alpha, -beta, -rc)
- Build metadata identifiers

Pre-releases do not carry stability guarantees unless explicitly stated.


---

## 10. Prohibited Practices

The following are prohibited:

- Changing public behavior without version increment.
- Removing public API without MAJOR increment (for stable APIs).
- Introducing ABI breaks in PATCH or MINOR (for stable ABI projects).
- Re-tagging released versions with different content.


---

## 11. Minimal Compliance Checklist

A repository is compliant if:

- Versioning scheme is declared.
- Compatibility meaning of version components is documented.
- Breaking changes align with version increments.
- Deprecation and removal align with version policy.
- Releases are tagged and reproducible.
