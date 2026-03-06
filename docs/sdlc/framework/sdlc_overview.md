# sdlc_overview

## 1. Purpose

This document provides an overview of the Software Development Lifecycle (SDLC) framework and describes its purpose, scope, and organization.

The SDLC framework defines the engineering governance, architectural principles, programming standards, documentation practices, and tooling conventions required to design, implement, and maintain reliable software systems.

------

## 2. Scope

The SDLC framework governs the development and maintenance of software systems produced under the engineering organization.

The framework applies to:

- engineering governance
- software architecture
- programming standards
- engineering practices
- documentation standards
- repository and tooling conventions

The SDLC framework provides the normative rules and guidance required to ensure consistency, maintainability, and long-term evolvability across software projects.

Project-specific design and implementation decisions remain the responsibility of individual projects, provided they comply with the rules defined by the SDLC.

------

## 3. Objectives

The SDLC framework exists to achieve the following objectives:

- establish consistent engineering practices across projects
- ensure long-term maintainability of software systems
- provide clear architectural and engineering guidance
- support safe evolution of software systems over time
- enable predictable collaboration across engineering teams

The framework emphasizes clarity, determinism, and maintainability in both engineering practice and documentation.

------

## 4. Structure of the SDLC Framework

The SDLC framework is organized into several domains, each addressing a specific aspect of software engineering governance.

### governance

Defines the policies and processes governing engineering practices and the evolution of the SDLC framework itself.

Examples include:

- engineering governance
- servicing and maintenance strategy
- SDLC document standards

### architecture

Defines architectural principles and system structure expectations for software projects.

Examples include:

- architecture layering
- component structure
- taxonomy definitions

### standards

Defines mandatory programming and engineering rules.

Standards are normative and MUST be followed by projects governed by the SDLC.

Examples include:

- C++ coding standard
- repository layout conventions

### guidelines

Provides recommended engineering practices.

Guidelines are advisory and SHOULD be followed unless there is a justified reason to deviate.

Examples include:

- C++ engineering guidelines
- engineering baseline guidance

### documentation

Defines standards for documenting software systems and source code.

Examples include:

- source code documentation standard
- project documentation requirements
- versioning policy

### tooling

Defines repository conventions and tooling expectations supporting SDLC practices.

Examples include:

- repository layout conventions
- automation guidelines

### templates

Provides reusable templates that assist projects in complying with SDLC documentation and governance requirements.

Templates are informational and do not define normative rules.

------

## 5. Authority of SDLC Documents

Documents within the SDLC framework have different levels of authority.

### normative documents

Normative documents define mandatory requirements.

Projects governed by the SDLC MUST comply with these documents.

Examples include:

- coding standards
- architectural rules
- governance policies

### informational documents

Informational documents provide guidance, explanations, or templates.

These documents do not impose mandatory requirements.

Examples include:

- engineering guidelines
- documentation templates

------

## 6. Evolution of the SDLC Framework

The SDLC framework evolves over time through a controlled amendment process.

Baseline documents define the canonical specification.

Changes to baseline documents SHOULD normally be introduced through amendment documents rather than directly modifying the baseline.

Amendments are applied according to the SDLC maintenance model defined in:

- servicing_and_maintenance_strategy
- sdlc_document_standard

Over time, amendments MAY be consolidated into updated baseline documents.

------

## 7. Relationship to Software Projects

The SDLC framework provides the governing structure for software projects but does not define the implementation details of individual systems.

Projects are expected to:

- follow the standards defined by the SDLC
- adopt architectural principles defined by the framework
- maintain documentation consistent with SDLC documentation standards
- use repository structures compatible with SDLC tooling conventions

Projects MAY extend the SDLC framework with additional project-specific documentation and guidance.

------

## 8. References

engineering_governance
servicing_and_maintenance_strategy
sdlc_document_standard