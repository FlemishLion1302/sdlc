# api_reference_generation

## 1. Purpose

This document defines requirements for generating API reference documentation from source code within SDLC-governed repositories.

API reference documentation provides a structured, navigable description of public APIs derived from source-level documentation.

The purpose of this specification is to ensure that reference documentation:

- is generated automatically from source code
- remains synchronized with the implementation
- is reproducible through automated tooling
- is consistently published alongside project documentation

------

## 2. Scope

This specification governs the generation of API reference documentation from source code.

It defines expectations for:

- documentation extraction from source comments
- generation tooling
- reproducibility of generated documentation
- CI integration
- publication of generated reference artifacts

This document does **not** define how APIs must be documented in source code.

Source-level documentation requirements are defined in:

- `source_code_documentation`

Repository documentation structure is defined in:

- `project_documentation`

------

## 3. Authority

This document forms part of the SDLC documentation governance domain.

Its authority derives from:

- `sdlc_framework_overview`
- `sdlc_governance`
- `documentation_governance`

Projects may introduce stricter generation policies but may not weaken the requirements defined here.

------

## 4. Documentation Source

API reference documentation **MUST** be generated from source code documentation.

The authoritative source for API documentation is therefore:

- documentation comments embedded in source code
- public interface definitions

Projects **MUST NOT** manually maintain duplicate API documentation in repository documentation.

Manual duplication leads to documentation drift and is prohibited.

------

## 5. Generation Tooling

Projects **SHOULD** use documentation generators capable of extracting structured documentation from source code.

Typical examples include:

- Doxygen
- Sphinx with language extensions
- language-specific documentation generators

The SDLC framework does not mandate a specific documentation tool.

However, the selected tool **MUST** support:

- structured documentation comments
- automated generation
- reproducible output

------

## 6. Generation Requirements

Documentation generation **SHOULD** be deterministic.

Running the documentation generation process multiple times with identical inputs should produce equivalent outputs.

Generation pipelines **SHOULD**:

- operate without manual intervention
- run as part of automated CI workflows
- produce stable documentation artifacts

------

## 7. Output Structure

Generated reference documentation **MUST** be placed in the repository documentation structure defined by `project_documentation`.

The standard location is:

```
docs/reference/
```

Rules:

- the directory is **generated content**
- files **MUST NOT** be manually edited
- generated content may be regenerated or replaced by tooling

------

## 8. Versioning

API reference documentation **SHOULD** correspond to specific project versions.

Projects are encouraged to generate documentation:

- for tagged releases
- for the current development branch

Published documentation should clearly identify the version or commit associated with the generated reference.

------

## 9. CI Integration

Projects **SHOULD** integrate documentation generation into their CI pipelines.

Typical CI workflows include:

- generating reference documentation during builds
- verifying documentation generation does not fail
- optionally publishing documentation artifacts

Projects may also:

- fail builds when generation fails
- enforce documentation coverage thresholds

------

## 10. Publication

Generated API reference documentation **SHOULD** be published alongside project documentation.

Common publication mechanisms include:

- static documentation sites
- repository-hosted documentation portals
- documentation artifacts attached to releases

Publication tooling is implementation-specific and outside the scope of this specification.

------

## 11. Documentation Consistency

Generated reference documentation must remain consistent with source code behavior.

The following practices are required:

- documentation must be regenerated when APIs change
- stale reference documentation must not be published
- documentation generation failures should be treated as build failures where practical

------

## 12. Guidance

Projects should favor generation workflows that:

- require minimal manual maintenance
- produce stable documentation output
- integrate easily with CI pipelines

Documentation generation pipelines should remain simple and reproducible.

------

## 13. Compliance Summary

A repository is compliant with this specification if:

- API reference documentation is generated from source
- generated documentation resides under `docs/reference`
- generation is reproducible
- documentation generation is integrated into project workflows

Failure to maintain reproducible reference documentation constitutes SDLC non-compliance.

------

## 14. Amendment and Evolution

Changes to this document follow the SDLC amendment model.

Amendment mechanics are defined in:

- `sdlc_governance`
- `sdlc_document_standard`

Amendments may later be consolidated into revised baseline documents.

------

## 15. References

- `sdlc_framework_overview`
- `sdlc_governance`
- `sdlc_document_standard`
- `documentation_governance`
- `project_documentation`
- `source_code_documentation`

