# Engineering Governance Repository

This repository is the authoritative source of engineering governance artifacts used by the SDLC framework.

It defines the standards, guidelines, architectural policies, documentation governance, and tooling conventions that govern software development within the organization.

Projects governed by the SDLC **MUST reference a tagged version of this repository** when adopting engineering governance.

Governance artifacts are versioned and immutable once released.

------

# SDLC Framework

The Software Development Lifecycle (SDLC) framework defines the governance structure used by this repository.

The framework describes:

- SDLC governance domains
- authority relationships between domains
- terminology used across SDLC specifications
- amendment and consolidation mechanics

The SDLC framework overview is defined in:

```
docs/sdlc/framework/sdlc_framework_overview.md
```

This document serves as the entry point for understanding the SDLC governance model.

------

# Governance Domains

Engineering governance artifacts are organized into several domains.

Each domain groups related specifications that govern a particular aspect of engineering practice.

## Language Standards

Defines mandatory programming standards and recommended engineering practices.

Examples include:

- C++ Coding Standard
- C++ Engineering Guidelines

Located under:

```
docs/sdlc/engineering/standards/
docs/sdlc/engineering/guidelines/
```

These specifications define language-level engineering practices.

------

## Architecture and Taxonomy

Defines system structure, component boundaries, and dependency rules.

Examples include:

- architectural layering rules
- platform taxonomy definitions
- dependency direction policies

Located under:

```
docs/sdlc/engineering/taxonomy/
docs/sdlc/engineering/layering/
```

These specifications define structural constraints for software systems.

------

## Documentation Governance

Defines requirements governing how software systems and APIs are documented.

Examples include:

- project documentation requirements
- source code documentation standards
- API reference generation requirements
- deprecation policy
- versioning policy

Located under:

```
docs/sdlc/documentation/
```

These specifications define how software systems describe and evolve their public contracts.

------

## Tooling Governance

Defines automation conventions and repository structure expectations.

Examples include:

- tooling integration expectations
- repository layout conventions
- automation reproducibility rules

Located under:

```
docs/sdlc/tooling/
```

These specifications define how engineering processes are automated and executed.

------

# Governance Hierarchy

The authority hierarchy governing SDLC domains is defined in:

```
docs/sdlc/framework/sdlc_framework_overview.md
```

Engineering governance documents operate within that hierarchy.

The lifecycle and amendment mechanics governing engineering specifications are defined in:

```
docs/sdlc/engineering/governance/engineering_governance.md
```

The servicing and consolidation strategy for SDLC documents is defined in:

```
docs/sdlc/framework/servicing_and_maintenance_strategy.md
```

------

# Repository Structure

The repository is organized according to the SDLC governance domains.

```
docs/sdlc/
├─ framework/
├─ engineering/
│  ├─ governance/
│  ├─ standards/
│  ├─ guidelines/
│  ├─ layering/
│  └─ taxonomy/
├─ documentation/
│  ├─ governance/
│  ├─ guidelines/
│  └─ policies/
├─ tooling/
└─ templates/
```

------

# Adoption by Projects

Projects adopting SDLC governance **MUST**:

1. Reference a tagged version of this repository (for example `v1.0.1`).
2. Declare the adopted governance version in their repository documentation.
3. Treat governance artifacts as immutable once adopted.

Projects **MUST NOT** reference floating branches such as `master`.

------

# Versioning Model

Governance artifacts evolve through versioned releases.

- Tags represent governance release points.
- Changes are introduced through amendment documents.
- Periodic consolidation revisions may update baseline documents.
- Projects should adopt governance versions intentionally rather than automatically.

------

# Immutability

Governance artifacts are immutable once released.

Changes to released governance specifications must occur through:

- versioned amendments, or
- formal consolidation revisions

Direct modification of previously released artifacts is prohibited.
