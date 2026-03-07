# sdlc_framework_overview

## 1. Purpose

This document provides an overview of the Software Development Lifecycle (SDLC) framework.

The SDLC framework defines the governed set of documents that establish engineering governance, architectural principles, programming standards, documentation practices, tooling conventions, and reusable templates used to develop and maintain software systems.

This document introduces the structure, terminology, and intent of the framework and serves as the primary entry point for understanding the SDLC.

------

## 2. Scope

The SDLC framework governs engineering practices for software systems developed under the organization.

The framework defines:

- the domains that organize SDLC documentation
- the types of documents used to define rules and guidance
- the authority relationships between SDLC domains
- the terminology used throughout the SDLC

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

# 4. SDLC Domain Model

The SDLC framework organizes its documents into **domains**.

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

Additional domains may be introduced as the SDLC framework evolves.

Within each domain, related documents may collectively form a **specification** describing a particular area of engineering practice.

For example, multiple documents in the standards and guidelines domains may together define a language engineering specification.

------

# 5. Domain Descriptions

## governance

The governance domain defines the policies and processes governing engineering practices and the evolution of the SDLC framework itself.

Governance documents establish the authority model for all other SDLC domains and define how engineering policy is created, amended, and maintained.

Examples include:

- engineering governance
- servicing and maintenance strategy

Governance documents are typically **normative**.

------

## architecture

The architecture domain defines structural constraints governing the organization of software systems and components.

Architecture documents describe system layering, component boundaries, platform abstraction models, and dependency direction rules.

Architecture rules constrain the structure of software systems and inform the standards and engineering practices applied by projects.

Architecture documents are typically **normative**.

------

## standards

The standards domain defines mandatory engineering and programming practices applied within the architectural constraints defined by the architecture domain.

Standards establish rules governing implementation practices such as:

- programming language usage
- repository conventions
- coding standards
- ABI stability policies

Standards documents are **normative**.

------

## guidelines

The guidelines domain provides recommended engineering practices.

Guidelines describe preferred approaches that support maintainability, consistency, and engineering quality.

Guidelines support the application of architecture and standards but do not define mandatory requirements.

Guidelines are advisory and may be deviated from when justified.

Guidelines documents are typically **informational**.

------

## documentation

The documentation domain defines rules and guidance governing how software systems, APIs, and engineering artifacts are documented.

These documents ensure that APIs, systems, and engineering artifacts are described consistently and clearly.

Documentation documents may contain normative requirements governing **documentation practices**, but they do not define engineering or architectural rules.

Documentation documents may be **normative or informational** depending on their purpose.

------

## tooling

The tooling domain defines repository conventions and automation practices supporting SDLC compliance.

Tooling documents describe how engineering processes are supported through repository structure and automation.

Tooling documents are typically **informational**.

------

## templates

The templates domain provides reusable artifacts that assist projects in complying with SDLC documentation and governance requirements.

Templates are informational and do not define normative rules.

------

# 6. Authority Relationships

Domains within the SDLC framework have defined authority relationships.

When conflicts arise between documents from different domains, the document belonging to the higher-authority domain prevails.

The SDLC authority hierarchy is:

```
governance
   ↓
architecture
   ↓
standards
   ↓
guidelines
   ↓
documentation
   ↓
tooling
   ↓
templates
```

Domains higher in the hierarchy define constraints that must be respected by documents in lower domains.

Normative engineering requirements are primarily defined by the **governance**, **architecture**, and **standards** domains.

The **guidelines** domain provides advisory practices intended to support the application of standards and architecture but does not define mandatory requirements.

The documentation, tooling, and templates domains support the application of SDLC rules but must remain consistent with higher-authority domains.

------

# 7. Authority Rules

The following rules clarify how the SDLC domain hierarchy is applied when interpreting SDLC documents.

## governance precedence

Governance documents define how engineering practices and the SDLC framework itself are governed.

Governance documents take precedence over all other SDLC domains.

------

## architecture precedence

Architecture documents define structural constraints on software systems.

Architecture rules take precedence over standards and guidelines.

------

## standards precedence

Standards define mandatory engineering and programming requirements.

Standards take precedence over guidelines.

------

## guidelines precedence

Guidelines provide recommended engineering practices.

Guidelines MUST remain consistent with architecture and standards and MUST NOT contradict normative requirements defined by higher-authority domains.

------

## documentation and tooling

Documentation, tooling, and templates support the application of SDLC rules.

These domains must remain consistent with governance, architecture, standards, and guidelines.

------

# 8. Evolution of the SDLC Framework

The SDLC framework evolves over time through a controlled revision process.

Baseline documents define the canonical specification for a given area of the framework.

Changes to baseline documents are typically introduced through amendment documents that revise specific rules without requiring routine modification of the baseline document.

Over time, amendments may be incorporated into updated baseline documents through periodic consolidation.

The detailed revision model governing SDLC documents is defined in:

- `engineering_governance`
- `sdlc_document_standard`

------

# 9. Relationship to Software Projects

The SDLC framework provides the governing structure for software projects but does not define the implementation details of individual systems.

Projects governed by the SDLC are expected to:

- comply with normative documents defined by the framework
- adopt architectural principles defined in the architecture domain
- follow programming standards defined in the standards domain
- maintain documentation consistent with SDLC documentation standards
- follow repository and tooling conventions defined by the framework

Projects may introduce additional project-specific documentation and guidance, provided those documents remain consistent with the SDLC framework.

------

# 10. Terminology

## SDLC Framework

The governed body of documents that defines the engineering governance, architectural principles, standards, guidelines, documentation practices, tooling conventions, and templates used to develop and maintain software systems.

------

## Domain

A distinct area of the SDLC framework grouping related documents by subject and authority.

Examples include governance, architecture, standards, guidelines, documentation, tooling, and templates.

------

## Specification

A governed collection of related SDLC documents that together define the rules, guidance, and supporting materials for a specific area of engineering practice.

A specification may include documents from multiple SDLC domains, such as standards, guidelines, and documentation.

Specifications often contain a **specification root document** that describes the scope and boundaries of the specification.

------

## Normative Document

A document that defines mandatory requirements.

Normative documents MUST be followed by repositories and projects governed by the SDLC.

------

## Informational Document

A document that provides explanation, guidance, examples, or templates.

Informational documents do not define mandatory requirements.

------

## Baseline Document

A document that defines the canonical specification for a rule set within the SDLC framework.

Baseline documents remain stable over time and are revised through amendments and periodic consolidation.

------

## Amendment

An additive document that revises one or more rules defined by a baseline document without requiring routine direct modification of the baseline.

------

## Amendment Precedence

The rule by which multiple amendments affecting the same rule are interpreted in chronological order, with the most recent applicable amendment taking precedence.

------

## Consolidation

The act of incorporating the current authoritative state of one or more amended rules into the corresponding baseline document.

------

## Rule

A statement defined by an SDLC document that expresses a requirement, prohibition, recommendation, or permission using the normative language defined by the SDLC.

------

## Rule Identifier

A stable identifier assigned to a rule to provide a persistent reference point for amendments and cross-references.

Rule identifiers SHOULD remain stable even if document structure or section numbering changes.

------

## Repository

A version-controlled source of truth containing a governed set of files and history.

------

## Repository Root

The top-level directory of a repository.

------

## Workspace

A higher-level development grouping that may contain one or more related projects or repositories.

------

## Workspace Root

The top-level directory of a workspace.

------

## Project

A distinct software deliverable, build target, or governed unit of engineering work within a workspace or repository context.

------

## Project Root

The top-level directory of a project.

------

# 11. References

- engineering_governance
- sdlc_document_standard
- servicing_and_maintenance_strategy
