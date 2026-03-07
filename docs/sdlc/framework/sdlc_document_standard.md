# sdlc_document_standard

## 1. Purpose

Defines the format, structure, authorship, and maintenance model for SDLC documents.

---

## 2. Scope

Applies to all normative and informational documents that form part of the SDLC framework.

---

## 3. Document Format

SDLC documents must be authored in Markdown.

The first line MUST be a level-1 heading containing the document title.

---

## 4. File Naming

Filenames must use `lower_snake_case`.

---

## 5. Document Identification

SDLC documents MUST include a stable document identifier.

The document identifier provides a persistent reference independent of filename or location.

The identifier SHOULD appear immediately after the document title.

Example:
```
# project_documentation

Document Identifier: SDLC-DOC-002
```
Document identifiers SHOULD remain stable across revisions, amendments, and consolidations.

---

## 6. Recommended Document Structure

Recommended structure:

1. Purpose  
2. Scope  
3. Authority  
4. Definitions (optional)  
5. Rules / Requirements  
6. Guidance (optional)  
7. Rationale (optional)  
8. References  

Documents may omit sections that are not applicable to their purpose.

---

## 7. Normative Language

RFC-2119 terminology MUST be used for normative requirements.

---

## 8. Rule Identification

Normative rules SHOULD include a stable rule identifier.

Example:
```
rule: cpp_ns_align
```
---

## 9. Baseline Documents

Baseline documents define canonical rules and remain stable.

Normative changes SHOULD normally be introduced through amendment documents.

---

## 10. Amendment Documents

Amendment documents revise baseline documents.

Amendment documents SHOULD identify the target document using the document identifier and the affected rule using the rule identifier.

Amendment filenames:
```
<base_document>_<rule_identifier>_<yyyy_mm_dd>.md
```
Example:
```
cpp_coding_standard_cpp_ns_align_2026_03_06.md
```
---

## 11. Amendment Precedence

When multiple amendments affect the same rule, the most recent amendment takes precedence.

---

## 12. Consolidation

Amendments may be incorporated into revised baseline documents during consolidation.

---

## 13. Authorship and Revision

Changes to SDLC documents must be introduced through pull requests and maintain consistency with related documents.

---

## 14. References

- sdlc_framework_overview
- engineering_governance

