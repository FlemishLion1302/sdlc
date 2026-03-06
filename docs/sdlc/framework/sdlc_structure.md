# sdlc_structure

## 1. Purpose

This document defines the structural organization of the Software Development Lifecycle (SDLC) framework and the relationships between its document domains.

The SDLC framework organizes its documents into domains representing distinct areas of engineering governance. This document describes those domains and establishes the authority relationships between them.

------

## 2. Scope

This document applies to all documents that form part of the SDLC framework.

It defines:

- the domains used to organize SDLC documents
- the purpose of each domain
- the authority relationships between domains

This document does not define the specific rules contained within each domain.

------

## 3. SDLC Domain Model

The SDLC framework organizes its documents into domains.

Each domain groups documents addressing a specific area of engineering governance.

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

Each domain contains documents relevant to its subject area.

------

## 4. Domain Descriptions

### governance

The governance domain defines the policies and processes governing engineering practices and the evolution of the SDLC framework itself.

Examples include engineering governance and servicing and maintenance strategy.

------

### architecture

The architecture domain defines structural principles governing the organization of software systems.

Architecture documents describe system layering, component structure, and architectural policies.

Architecture rules constrain the structure of software systems and inform the standards and practices applied by projects.

------

### standards

The standards domain defines mandatory engineering and programming rules.

Standards are normative documents that establish requirements projects must follow.

Examples include programming language standards and repository conventions.

------

### guidelines

The guidelines domain provides recommended engineering practices.

Guidelines describe preferred approaches that support maintainability, consistency, and engineering quality.

Guidelines are advisory and may be deviated from when justified.

------

### documentation

The documentation domain defines rules governing how software systems and source code are documented.

These documents ensure that APIs, systems, and engineering artifacts are described consistently and clearly.

------

### tooling

The tooling domain defines repository conventions and automation practices supporting SDLC compliance.

Tooling documents describe how engineering processes are supported through repository structure and automation.

------

### templates

The templates domain provides reusable artifacts that assist projects in complying with SDLC documentation and governance requirements.

Templates are informational and do not define normative rules.

------

## 5. Authority Relationships

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

------

## 6. Authority Rules

The following rules apply when interpreting SDLC documents.

### governance precedence

Governance documents define how engineering practices and the SDLC framework itself are governed.

Governance documents take precedence over all other SDLC domains.

------

### architecture precedence

Architecture documents define structural constraints on software systems.

Architecture rules take precedence over standards and guidelines.

------

### standards precedence

Standards define mandatory engineering and programming requirements.

Standards take precedence over guidelines.

------

### guidelines precedence

Guidelines provide recommended engineering practices.

Guidelines do not override standards or architecture rules.

------

### documentation and tooling

Documentation and tooling domains support the application of SDLC rules.

These domains must remain consistent with governance, architecture, and standards.

------

## 7. Cross-Domain Consistency

Documents within the SDLC framework must remain internally consistent.

When conflicts arise between documents:

1. the document belonging to the higher-authority domain prevails
2. the conflicting document should be revised to restore consistency

Cross-references between related documents should be maintained to clarify relationships between domains.

------

## 8. References

sdlc_overview
sdlc_glossary
sdlc_governance
sdlc_document_standard