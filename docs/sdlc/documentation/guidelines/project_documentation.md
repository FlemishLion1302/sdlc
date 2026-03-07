# project_documentation

## 1. Purpose

This document defines the minimum **repository-level documentation requirements** for any project governed by the SDLC framework.

Documentation is considered part of the software product and must be versioned, reviewed, and maintained alongside source code.

This standard defines:

- required repository-level documents
- required `docs/` directory structure
- documentation ownership responsibilities
- CI expectations for documentation generation and publication

The goal of this policy is to ensure that project documentation remains reliable, discoverable, and aligned with system behavior.

------

## 2. Scope

This standard applies to all repositories governed by the SDLC framework.

It defines minimum expectations for:

- repository-level documentation
- project documentation structure
- generated reference documentation
- ABI documentation where applicable

This document governs **project and repository documentation**, not source-code documentation.

Documentation embedded within source code is governed by:

- `source_code_documentation`

Automated API documentation generation is governed by:

- `api_reference_generation`

Projects may exceed the requirements defined here but must not fall below them.

------

## 3. Authority

This document forms part of the SDLC documentation governance domain.

Its authority derives from:

- `sdlc_framework_overview`
- `sdlc_governance`
- `documentation_governance`

Engineering standards may impose additional documentation requirements but may not weaken the requirements defined here.

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

# 5. Mandatory Repository-Level Files

Every governed repository **MUST** contain the following root-level files.

## 5.1 README.md

The README **MUST** provide:

- project name
- one-paragraph description (what it is, not how it works)
- build instructions
- basic usage example
- license reference
- link to full documentation (`docs/`)

The README **MUST NOT** attempt to replace proper documentation.

------

## 5.2 LICENSE

Repositories **MUST** include a clearly defined project license.

------

## 5.3 CHANGELOG.md

Repositories **MUST** include a versioned changelog documenting:

- Added
- Changed
- Deprecated
- Removed
- Fixed

Alignment with the `versioning_policy` is recommended.

------

# 6. Required `docs/` Structure

Every implementation repository **MUST** contain a `docs/` directory.

Minimum required structure:

```
docs/
├─ index.md
├─ overview/
├─ usage/
├─ design/
├─ dev/
├─ reference/   (generated; do not hand-edit)
└─ abi/         (generated if applicable)
```

------

## 6.1 index.md

The documentation landing page **MUST** describe:

- system purpose
- intended audience
- navigation overview

------

# 7. Documentation Categories

## 7.1 Overview Documentation

Location:

```
docs/overview/
```

Purpose:

Explain what the system is and how it is structured.

Typical contents:

- architecture overview
- module boundaries
- dependency rules
- high-level diagrams

Overview documentation is conceptual and relatively stable.

------

## 7.2 Usage Documentation

Location:

```
docs/usage/
```

Purpose:

Explain how to use the system.

Typical contents:

- getting started guide
- configuration
- example workflows
- integration patterns

This documentation is consumer-facing.

------

## 7.3 Design Documentation

Location:

```
docs/design/
```

Purpose:

Explain key technical decisions and constraints.

Typical contents:

- error model
- threading model
- memory ownership rules
- extension mechanisms

Design documentation explains *why*, not only *what*.

------

## 7.4 Developer Documentation

Location:

```
docs/dev/
```

Purpose:

Support contributors and maintainers.

Typical contents:

- build matrix
- toolchain setup
- testing strategy
- CI pipeline overview
- contribution guidelines

------

## 7.5 Reference Documentation (Generated)

Location:

```
docs/reference/
```

This directory contains generated API reference documentation.

Rules:

- reference documentation **MUST** be generated from source
- generated documentation **MUST NOT** be manually edited
- generation **SHOULD** be reproducible through CI
- documentation **SHOULD** correspond to a specific version or tag

Generation requirements are defined in `api_reference_generation`.

------

## 7.6 ABI Documentation (If Applicable)

Location:

```
docs/abi/
```

Required only for:

- shared libraries
- components with ABI stability guarantees

This directory **MUST** contain:

- ABI baseline snapshots
- ABI comparison reports

ABI documentation **MUST** be generated by tooling and **MUST NOT** be manually written.

------

# 8. CI Expectations

Projects **SHOULD**:

- generate API reference documentation automatically in CI
- publish generated documentation on release
- fail builds when documentation generation fails
- optionally enforce documentation coverage for public APIs

Projects with ABI guarantees **SHOULD**:

- generate ABI baselines on release
- compare ABI changes in CI for stable branches

------

# 9. Ownership and Maintenance

Documentation is owned by the same maintainers responsible for the code.

The following rules apply:

- code changes that alter public behavior **MUST** update documentation
- breaking changes **MUST** update `CHANGELOG.md`
- deprecated features **MUST** be documented

API documentation within source code must follow the requirements defined in `source_code_documentation`.

------

# 10. Guidance

This standard intentionally avoids:

- mandating a specific documentation toolchain
- mandating diagram tools
- over-specifying formatting

Projects **MAY** adopt additional documentation conventions that suit their needs.

------

# 11. Compliance Summary

A repository is compliant with this standard if:

- mandatory root documents exist
- the required `docs/` structure exists
- reference documentation is generated
- documentation is version-controlled
- documentation evolves with code

Failure to meet these requirements constitutes SDLC non-compliance.

------

# 12. Amendment and Evolution

Changes to this document follow the SDLC amendment model.

Amendments **SHOULD** normally be introduced through amendment documents rather than modifying the baseline directly.

Amendment mechanics are defined in:

- `sdlc_governance`
- `sdlc_document_standard`

Over time, amendments may be consolidated into revised baseline editions.

------

# 13. References

- `sdlc_framework_overview`
- `sdlc_governance`
- `sdlc_document_standard`
- `servicing_and_maintenance_strategy`
- `documentation_governance`
- `versioning_policy`
- `deprecation_policy`

