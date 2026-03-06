# sdlc_document_standard

## 1. Purpose

This document defines the format, structure, authorship, and maintenance model for documents within the Software Development Lifecycle (SDLC) framework.

Consistent document structure ensures that SDLC specifications are predictable to read, easy to maintain, and evolvable through controlled amendments.

------

## 2. Scope

This standard applies to all documents that form part of the SDLC framework.

This includes documents governing:

- engineering governance
- architecture
- programming standards
- engineering guidelines
- documentation standards
- tooling conventions
- SDLC templates

Project-specific documentation is outside the scope of this standard.

------

## 3. Document Format

SDLC documents MUST be authored in Markdown (`.md`).

The first line of every SDLC document MUST be a level-1 Markdown heading identifying the document title.

Example:

```markdown
# sdlc_document_standard
```

Document versioning is governed by Git history and repository tags.
Version numbers MUST NOT be embedded in SDLC documents.

------

## 4. File Naming

SDLC document filenames MUST use `lower_snake_case`.

File names MUST consist only of:

- lowercase letters
- digits
- underscores

Hyphens, spaces, and camelCase MUST NOT be used.

Examples:

```
sdlc_overview.md
engineering_governance.md
servicing_and_maintenance_strategy.md
cpp_coding_standard.md
repo_layout_conventions.md
```

This aligns SDLC documentation with the repository naming conventions used for project source files and other engineering artifacts.

------

## 5. Recommended Document Structure

SDLC documents SHOULD follow a consistent section structure.

The recommended structure is:

```
1. Purpose
2. Scope
3. Authority
4. Definitions (optional)
5. Rules / Requirements
6. Guidance (optional)
7. Rationale (optional)
8. References
```

Not all sections are required for every document.
When present, sections SHOULD appear in this order.

------

## 6. Normative Language

Normative requirements MUST use RFC-2119 terminology.

The following keywords have defined meaning:

| Keyword  | Meaning               |
| -------- | --------------------- |
| MUST     | Mandatory requirement |
| MUST NOT | Prohibited            |
| SHOULD   | Strong recommendation |
| MAY      | Optional              |

Normative statements SHOULD be written clearly and unambiguously.

------

## 7. Rule Identification

Normative rules SHOULD include a stable rule identifier.

Rule identifiers provide a persistent reference point for amendments.

Rule identifiers SHOULD use `lower_snake_case`.

Example:

```
rule: cpp_ns_align
```

Rule identifiers SHOULD remain stable even if document sections are reorganized.

------

## 8. Baseline Documents

Baseline documents define the canonical specification for SDLC rules.

Baseline documents SHOULD remain stable and SHOULD NOT be modified for routine clarifications or incremental improvements.

Changes SHOULD normally be introduced through amendment documents.

Maintaining stable baseline documents ensures that rule evolution can be tracked independently of the original specification.

------

## 9. Amendment Documents

Amendment documents modify baseline documents without editing them directly.

An amendment document MUST:

- identify the baseline document it modifies
- identify the rule affected
- provide the amended rule wording
- provide rationale when appropriate

Amendment filenames SHOULD follow this pattern:

```
<base_document>_<rule_identifier>_<yyyy_mm_dd>.md
```

Example:

```
cpp_coding_standard_cpp_ns_align_2026_03_06.md
```

Amendments allow the SDLC framework to evolve without repeatedly modifying baseline documents.

------

## 10. Amendment Precedence

When multiple amendments affect the same rule, they are interpreted in chronological order.

The most recent amendment affecting a rule takes precedence over earlier amendments.

Earlier amendments remain part of the historical record but are superseded by later amendments.

Chronological ordering is determined by Git history and amendment date.

------

## 11. Periodic Consolidation

Over time, amendments may accumulate for a given document.

When this occurs, the baseline document MAY be revised to incorporate the current authoritative wording of affected rules.

During consolidation:

- amendments are integrated into the baseline
- superseded amendments become historical artifacts
- rule identifiers remain stable

Periodic consolidation restores readability while preserving the historical evolution of the rule set.

------

## 12. Authorship and Revision

All SDLC documents are maintained through Git.

Changes to SDLC documents MUST:

- be introduced through pull requests
- include clear commit messages describing the change
- update related rules or references when required

Normative rule changes SHOULD be introduced through amendment documents unless the change occurs as part of a consolidation revision.

------

## 13. References

engineering_governance
servicing_and_maintenance_strategy