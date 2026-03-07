# Deprecation Policy Standard

## 1. Purpose

This document defines how SDLC-governed projects deprecate and remove public APIs.

Deprecation provides a controlled migration path for consumers while maintaining
stability guarantees appropriate to the project’s declared API/ABI policy.

Deprecation is mandatory for any removal of public surface unless the component
is explicitly declared unstable.


---

## 2. Scope

This policy applies to:

- Public classes
- Public functions
- Public enums and enumerators
- Public typedefs / aliases
- Exported C interfaces
- Public macros

It does NOT apply to:
- Private/internal code
- Experimental APIs explicitly marked unstable


---

## 3. Stability Prerequisite

Before applying this policy, the project MUST declare:

- Whether the API is considered stable
- Whether the ABI is considered stable (if applicable)

If API is declared unstable, deprecation may be skipped, but removal MUST still
be documented in CHANGELOG.md.


---

## 4. Deprecation Lifecycle

The standard lifecycle for a stable API is:

### Phase 1 — Active

- Fully supported.
- No warnings.

### Phase 2 — Deprecated

- Marked as deprecated in source.
- Documented as deprecated in reference docs.
- Migration path documented.
- Still functional.
- May emit compile-time warnings.

### Phase 3 — Removed

- Removed from public API.
- Documented in CHANGELOG.md under “Removed”.
- Versioning must reflect the removal according to the project’s versioning policy.


---

## 5. Minimum Deprecation Requirements

When deprecating a public API, the following are REQUIRED:

1. Source annotation (e.g., `[[deprecated]]` in C++ where applicable).
2. Documentation update indicating:
   - The API is deprecated.
   - Since which version.
   - Recommended replacement.
3. CHANGELOG.md entry under “Deprecated”.
4. Clear migration path.

Deprecation without migration guidance is prohibited.


---

## 6. Deprecation Duration

Projects MUST define a minimum supported deprecation window.

Pragmatic defaults:

- Patch releases: no removals.
- Minor releases: deprecations allowed, no removals.
- Major releases: removals allowed.

Alternative policies are allowed but MUST be documented.


---

## 7. ABI Considerations

For components with Stable ABI:

- Removing or changing a deprecated symbol in a compatible release line is prohibited.
- Deprecated symbols MUST remain ABI-compatible until the declared compatibility boundary ends.
- Removal requires a version boundary consistent with ABI policy.

If ABI is unstable, removal may occur without ABI guarantees, but must still
follow documentation and versioning rules.


---

## 8. Documentation Requirements

Reference documentation MUST:

- Clearly mark deprecated entities.
- Display deprecation notes prominently.
- Preserve documentation of deprecated APIs until removal.

Deprecated APIs MUST NOT silently disappear from reference documentation
without version boundary justification.


---

## 9. Prohibited Practices

The following are prohibited:

- Silent removal of public APIs.
- Deprecation without migration path.
- Changing behavior of deprecated APIs in incompatible ways.
- Marking APIs deprecated as a substitute for proper versioning discipline.


---

## 10. CI and Review Expectations

Projects SHOULD:

- Enable compiler warnings for deprecated usage internally.
- Track deprecated API usage within the repository.
- Prevent introduction of new internal dependencies on deprecated APIs.

Code review must treat public removal without prior deprecation as a policy violation
unless the API is explicitly unstable.


---

## 11. Minimal Compliance Checklist

A repository is compliant if:

- Public APIs are not removed without prior deprecation (for stable APIs).
- Deprecations are annotated in source.
- Migration paths are documented.
- CHANGELOG reflects deprecations and removals.
- Versioning aligns with removal events.
