# sdlc_document_standard

## 1. Purpose

This document defines the format, structure, authorship, and maintenance model for documents within the Software Development Lifecycle (SDLC) framework.

Consistent document structure ensures that SDLC specifications are predictable to read, easy to maintain, and capable of evolving through controlled amendments.

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

SDLC documents must be authored in Markdown (`.md`).

The first line of every SDLC document must be a level-1 Markdown heading identifying the document title.

Example:

```markdown
# sdlc_document_standard
```

Document versioning is governed by repository history.
Version numbers must not be embedded in SDLC documents.

------

## 4. File Naming

SDLC document filenames must use `lower_snake_case`.

File names must consist only of:

- lowercase letters
- digits
- underscores

Hyphens, spaces, and camelCase must not be used.

Examples:

```
sdlc_overview.md
engineering_governance.md
servicing_and_maintenance_strategy.md
cpp_coding_standard.md
repo_layout_conventions.md
```

This aligns SDLC documentation with the repository naming conventions used for project source files and engineering artifacts.

------

## 5. Recommended Document Structure

SDLC documents should follow a consistent section structure.

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
When present, sections should appear in this order.

------

## 6. Normative Language

Normative requirements must use RFC-2119 terminology.

The following keywords have defined meaning:

| Keyword  | Meaning               |
| -------- | --------------------- |
| MUST     | Mandatory requirement |
| MUST NOT | Prohibited            |
| SHOULD   | Strong recommendation |
| MAY      | Optional              |

Normative statements should be written clearly and unambiguously.

------

## 7. Rule Identification

Normative rules should include a stable rule identifier.

Rule identifiers provide a persistent reference point for amendments and cross-references.

Rule identifiers should use `lower_snake_case`.

Example:

```
rule: cpp_ns_align
```

Rule identifiers should remain stable even if document structure or section numbering changes.

------

## 8. Baseline Documents

Baseline documents define the canonical specification for rules within the SDLC framework.

Baseline documents should remain stable and should not be modified for routine clarifications or incremental improvements.

Changes are typically introduced through amendment documents.

Maintaining stable baseline documents allows rule evolution to be tracked independently from the original specification.

------

## 9. Amendment Documents

Amendment documents revise baseline documents without editing them directly.

An amendment document must:

- identify the baseline document it modifies
- identify the affected rule
- provide the amended rule wording
- describe the rationale for the revision when appropriate

Amendment filenames should follow this pattern:

```
<base_document>_<rule_identifier>_<yyyy_mm_dd>.md
```

Example:

```
cpp_coding_standard_cpp_ns_align_2026_03_06.md
```

Amendments allow the SDLC framework to evolve incrementally while preserving the stability of baseline documents.

------

## 10. Amendment Precedence

When multiple amendments affect the same rule, they are interpreted in chronological order.

The most recent amendment affecting a rule takes precedence over earlier amendments.

Earlier amendments remain part of the historical record but are superseded by later amendments.

Chronological ordering is determined by repository history.

------

## 11. Consolidation

Over time, amendments may accumulate for a given baseline document.

When this occurs, the baseline document may be revised to incorporate the current authoritative wording of affected rules.

During consolidation:

- amendments are integrated into the baseline document
- superseded amendments become historical artifacts
- rule identifiers remain stable

Consolidation restores readability while preserving the historical evolution of the rule set.

------

## 12. Authorship and Revision

SDLC documents are maintained through version control.

Changes to SDLC documents must:

- be introduced through pull requests
- include clear commit messages describing the change
- maintain consistency with related SDLC documents

Normative rule changes should normally be introduced through amendment documents unless the change occurs as part of a consolidation revision.

------

## 13. References

sdlc_overview
sdlc_structure
sdlc_glossary
sdlc_governance