# documentation_governance

## 1. Purpose

This document defines governance requirements for documentation within SDLC-governed projects.

Documentation is treated as a first-class engineering artifact.
It defines public contracts, communicates stability guarantees, and enables automated reference generation.

Documentation governance ensures that documentation remains consistent with system behavior and evolves alongside source code.

------

## 2. Scope

This document governs documentation expectations for SDLC-governed projects.

It defines minimum requirements for:

- repository-level documentation
- source code documentation of public APIs
- automated API reference generation
- documentation lifecycle policies including deprecation and versioning

This document does **not** define language-specific coding practices.
Those are governed by language standards such as the C++ Coding Standard.

------

## 3. Authority

Documentation governance forms part of the SDLC governance framework.

This document derives its authority from:

- `sdlc_framework_overview`
- `sdlc_governance`

Engineering standards may impose additional documentation requirements, but they may not weaken the minimum requirements defined here.

------

## 4. Normative Language

Normative language follows the definitions established in the SDLC document standard.

The keywords **MUST**, **MUST NOT**, **SHOULD**, and **MAY** have the meanings defined in RFC-2119.

These keywords indicate requirement strength:

| Keyword  | Meaning               |
| -------- | --------------------- |
| MUST     | Mandatory requirement |
| MUST NOT | Prohibited            |
| SHOULD   | Strong recommendation |
| MAY      | Optional behavior     |

------

## 5. Documentation Governance Requirements

### 5.1 Documentation as a Product Artifact

Documentation **MUST** be treated as part of the software product.

Documentation **MUST**:

- be version-controlled alongside source code
- evolve with the behavior it describes
- accurately represent system contracts

Projects **MUST NOT** allow documentation to diverge from implemented behavior.

------

### 5.2 Documentation Availability

Projects **MUST** provide documentation sufficient for:

- users
- integrators
- maintainers
- contributors

Documentation **SHOULD** include:

- system overview
- usage instructions
- design or architecture explanation
- developer guidance

------

### 5.3 Documentation Reproducibility

Documentation generation **SHOULD** be deterministic and reproducible.

When documentation is generated from source code or annotations, the generation process **SHOULD** be automated and reproducible through CI.

------

### 5.4 Documentation and Compatibility Guarantees

Documentation **MUST** accurately communicate compatibility guarantees.

When projects expose public APIs, documentation **MUST** align with:

- versioning policy
- deprecation policy
- ABI stability policy (when applicable)

Documentation describing compatibility guarantees **MUST** reflect actual project behavior.

------

## 6. Documentation Governance Artifacts

The following documents define specific documentation governance requirements.

### 6.1 Project Documentation

```
project_documentation.md
```

Defines:

- required repository-level documentation
- expected `docs/` directory structure
- categories of project documentation
- CI expectations for documentation publishing

------

### 6.2 Source Code Documentation

```
source_code_documentation.md
```

Defines requirements for documentation of public APIs, including:

- required API documentation elements
- contract documentation expectations
- documentation of thread-safety and error behavior

------

### 6.3 API Reference Generation

```
api_reference_generation.md
```

Defines requirements for generating API reference documentation from source code.

This includes:

- generation requirements
- output location conventions
- reproducibility expectations
- public surface inclusion rules

------

### 6.4 Deprecation Policy

```
deprecation_policy.md
```

Defines the lifecycle for deprecating APIs, including:

- required deprecation phases
- migration guidance expectations
- compatibility considerations
- relationship to versioning guarantees

------

### 6.5 Versioning Policy

```
versioning_policy.md
```

Defines versioning expectations for SDLC-governed projects.

This includes:

- version numbering schemes
- compatibility guarantees associated with version components
- release tagging expectations
- alignment with API stability and deprecation lifecycle

------

## 7. Guidance

Projects **MAY** exceed the documentation requirements defined in this specification.

Additional documentation such as tutorials, architecture deep-dives, or operational guides is encouraged.

Projects **MUST NOT** fall below the minimum documentation requirements defined here.

------

## 8. Amendment and Evolution

Changes to this document follow the SDLC amendment model.

Amendments **SHOULD** be introduced through amendment documents rather than direct modification of the baseline.

Amendment mechanics are defined in:

- `sdlc_governance`
- `sdlc_document_standard`

Over time, amendments may be consolidated into revised baseline editions.

------

## 9. Relationship to Other SDLC Domains

Documentation governance operates alongside other SDLC governance domains.

- Language standards define how APIs are implemented.
- Documentation governance defines how APIs are described and published.
- Tooling governance defines how documentation generation and automation are executed.

Together these form the documentation lifecycle:

```text
design → implementation → documentation → generation → publication → evolution
```

------

## 10. References

- `sdlc_framework_overview`
- `sdlc_governance`
- `sdlc_document_standard`
- `servicing_and_maintenance_strategy`

