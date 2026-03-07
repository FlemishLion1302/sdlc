# sdlc_framework_overview

Document Identifier: SDLC-FRM-001

## 1. Purpose

This document provides an overview of the Software Development Lifecycle (SDLC) framework.

The SDLC framework defines the governed set of documents that establish engineering governance, architectural principles, programming standards, documentation practices, tooling conventions, and reusable templates used to develop and maintain software systems.

This document introduces the structure, terminology, and intent of the framework and serves as the primary entry point for understanding the SDLC.

---

## 2. Scope

The SDLC framework governs engineering practices for software systems developed under the organization.

The framework defines:

- the domains that organize SDLC documentation
- the types of documents used to define rules and guidance
- the authority relationships between SDLC domains
- the terminology used throughout the SDLC

Project-specific design and implementation decisions remain the responsibility of individual projects, provided those projects comply with the normative documents defined by the SDLC framework.

---

## 3. Objectives

The SDLC framework exists to achieve the following objectives:

- establish consistent engineering practices across projects
- ensure long-term maintainability of software systems
- provide clear architectural and engineering guidance
- support controlled evolution of engineering standards
- enable predictable collaboration across engineering teams

---

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
 template
```
Additional domains may be introduced as the SDLC framework evolves.

---

## Domains vs Specifications

A **domain** groups documents by subject area and authority within the SDLC framework.

A **specification** is a governed collection of related documents that together define rules and guidance for a particular engineering concern.

Specifications may include documents from multiple domains.

Example:
```
C++ engineering specification

standards domain:
 cpp_coding_standard

guidelines domain:
 cpp_engineering_guidelines
```
Specifications defined within the SDLC framework **SHOULD define a specification root document**.

The specification root document serves as the structural entry point for the specification and identifies the governed documents that collectively define that specification.

---

# 5. Domain Descriptions

## governance

Defines policies and processes governing engineering practices and SDLC evolution.

Governance documents are typically **normative**.

Examples:

- engineering_governance
- servicing_and_maintenance_strategy

---

## architecture

Defines structural constraints governing the organization of software systems.

Architecture documents are typically **normative**.

Examples:

- architecture_layering
- taxonomy documents

---

## standards

Defines mandatory engineering and programming practices.

Standards documents are **normative**.

Examples:

- cpp_coding_standard

---

## guidelines

Defines recommended engineering practices.

Guidelines are **informational** and advisory.

Examples:

- cpp_engineering_guidelines

---

## documentation

Defines rules governing documentation practices for software systems.

Documentation documents may be **normative or informational**.

Examples:

- documentation_governance
- project_documentation

---

## tooling

Defines repository conventions and automation practices.

Tooling documents are typically **informational**.

---

## templates

Provides reusable artifacts that assist projects in complying with SDLC requirements.

Templates are informational.

---

# 6. Authority Relationships

When conflicts arise between domains, the document belonging to the higher-authority domain prevails.
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
Higher domains constrain lower domains.

---

# 7. Evolution of the SDLC Framework

Baseline documents define the canonical specification for a given rule set.

Changes to baseline documents are introduced through **amendments**.

Amendments modify rules without directly editing the baseline document.

Over time, amendments may be incorporated into updated baseline documents through **consolidation**.

Detailed revision mechanics are defined in:

- engineering_governance
- sdlc_document_standard

---

# 8. Relationship to Software Projects

Projects governed by the SDLC must:

- comply with normative SDLC documents
- follow architectural principles
- apply programming standards
- maintain documentation consistent with SDLC documentation standards
- follow tooling conventions

Projects may introduce additional project-specific documentation provided it remains consistent with the SDLC framework.

---

# 9. Terminology

## Domain

A distinct area of the SDLC framework grouping related documents by subject and authority.

---

## Specification

A governed collection of related SDLC documents that together define rules and guidance for a specific engineering concern.

---

## Specification Root Document

A document that defines the scope and structure of a specification.

It identifies the documents that collectively constitute the specification.

---

## Normative Document

A document defining mandatory requirements.

---

## Informational Document

A document providing guidance, explanation, examples, or templates.

---

## Baseline Document

A document defining the canonical specification for a rule set.

---

## Amendment

A document that revises rules defined by a baseline document.

---

## Consolidation

Integration of amended rules into a revised baseline document.

---

## Rule

A normative statement expressed using RFC-2119 language.

---

## Rule Identifier

A stable identifier assigned to a rule.

---

## Document Identifier

A stable identifier assigned to an SDLC document.

Document identifiers remain stable even if document filenames or locations change.

---

# 10. References

- engineering_governance
- sdlc_document_standard
- servicing_and_maintenance_strategy

