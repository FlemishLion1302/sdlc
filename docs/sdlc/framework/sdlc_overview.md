# sdlc_overview

## 1. Purpose

This document provides an overview of the Software Development Lifecycle (SDLC) framework.

The SDLC framework defines the governed set of documents that establish engineering governance, architectural principles, programming standards, documentation practices, tooling conventions, and reusable templates used to develop and maintain software systems.

This document introduces the structure and intent of the framework and serves as the primary entry point for understanding the SDLC.

------

## 2. Scope

The SDLC framework governs engineering practices for software systems developed under the organization.

The framework defines:

- the domains that organize SDLC documentation
- the types of documents used to define rules and guidance
- the relationship between the SDLC framework and individual software projects

Project-specific design and implementation decisions remain the responsibility of individual projects, provided those projects comply with the normative documents defined by the SDLC framework.

------

## 3. Objectives

The SDLC framework exists to achieve the following objectives:

- establish consistent engineering practices across projects
- ensure long-term maintainability of software systems
- provide clear architectural and engineering guidance
- support controlled evolution of engineering standards
- enable predictable collaboration across engineering teams

The framework emphasizes clarity, determinism, and maintainability in both engineering practice and documentation.

------

## 4. Structure of the SDLC Framework

The SDLC framework organizes its documents into domains.

Each domain groups documents addressing a specific aspect of engineering governance.

The principal SDLC domains are:

```
governance
architecture
standards
guidelines
documentation
tooling
templates
```

Each domain serves a distinct role within the framework.

### governance

Defines the policies and processes governing engineering practices and the evolution of the SDLC framework.

Examples include engineering governance and servicing and maintenance strategy.

### architecture

Defines structural principles governing the organization of software systems.

Architecture documents describe system layering, component structure, and architectural policies.

### standards

Defines mandatory engineering and programming rules.

Standards are normative documents that must be followed by projects governed by the SDLC.

### guidelines

Provides recommended engineering practices.

Guidelines are advisory and may be deviated from when justified.

### documentation

Defines rules governing the documentation of software systems and source code.

Documentation standards ensure that systems and APIs are described consistently and accurately.

### tooling

Defines repository conventions and automation guidance supporting SDLC practices.

### templates

Provides reusable artifacts that assist projects in complying with SDLC documentation and governance requirements.

Templates are informational and do not define normative rules.

------

## 5. Authority of SDLC Documents

Documents within the SDLC framework may be either normative or informational.

Normative documents define mandatory requirements that must be followed by projects governed by the SDLC.

Informational documents provide explanation, guidance, or reusable artifacts that assist in applying the framework.

The authority relationships between SDLC domains are defined in `sdlc_structure`.

------

## 6. Evolution of the SDLC Framework

The SDLC framework evolves over time through a controlled revision process.

Baseline documents define the canonical specification for a given area of the framework.

Changes to baseline documents are typically introduced through amendment documents that revise specific rules without requiring routine modification of the baseline document.

Over time, amendments may be incorporated into updated baseline documents through periodic consolidation.

The detailed revision model governing SDLC documents is defined in `sdlc_governance` and `sdlc_document_standard`.

------

## 7. Relationship to Software Projects

The SDLC framework provides the governing structure for software projects but does not define the implementation details of individual systems.

Projects governed by the SDLC are expected to:

- comply with normative documents defined by the framework
- adopt architectural principles defined in the architecture domain
- follow programming standards defined in the standards domain
- maintain documentation consistent with SDLC documentation standards
- follow repository and tooling conventions defined by the framework

Projects may introduce additional project-specific documentation and guidance, provided those documents remain consistent with the SDLC framework.

------

## 8. References

sdlc_glossary
sdlc_structure
sdlc_governance
sdlc_document_standard