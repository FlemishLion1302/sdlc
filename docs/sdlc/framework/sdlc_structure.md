# sdlc_structure

## 1. Purpose

This document defines the structural organization of the Software Development Lifecycle (SDLC) framework and the relationships between its constituent domains.

The SDLC framework is composed of multiple document domains addressing different aspects of software engineering governance.
This document clarifies the purpose and authority of each domain.

------

## 2. Scope

This document applies to all documents that form part of the SDLC framework.

It defines:

- the structural domains of the framework
- the purpose of each domain
- the authority relationships between domains

This document does not define the specific rules contained within each domain.

------

## 3. SDLC Domain Model

The SDLC framework is organized into a set of domains representing distinct areas of engineering governance.

The principal SDLC domains are:

```id="cfe45c"
governance
architecture
standards
guidelines
documentation
tooling
templates
```

Each domain defines rules or guidance relevant to a specific aspect of software development.

------

## 4. Domain Descriptions

### governance

The governance domain defines the policies and processes that control engineering practices and the evolution of the SDLC framework itself.

Governance documents establish how engineering rules are authored, revised, and maintained.

Examples include:

- engineering governance
- servicing and maintenance strategy
- sdlc document standard

------

### architecture

The architecture domain defines structural principles governing the organization of software systems.

Architecture documents define rules regarding system structure, component organization, and architectural layering.

Examples include:

- architecture layering
- taxonomy definitions
- architectural policies

------

### standards

The standards domain defines mandatory engineering and programming rules.

Standards are normative and MUST be followed by projects governed by the SDLC.

Examples include:

- C++ coding standard
- repository layout conventions

------

### guidelines

The guidelines domain provides recommended engineering practices.

Guidelines are advisory and SHOULD be followed unless there is a justified reason to deviate.

Examples include:

- C++ engineering guidelines
- engineering baseline guidance

------

### documentation

The documentation domain defines rules governing software documentation and API description.

These documents define requirements for documenting code, systems, and project artifacts.

Examples include:

- source code documentation standard
- project documentation requirements
- versioning policy

------

### tooling

The tooling domain defines repository conventions and automation guidance supporting SDLC practices.

Examples include:

- repository layout conventions
- tooling and automation guidance

------

### templates

The templates domain provides reusable artifacts intended to assist projects in complying with SDLC documentation and governance requirements.

Templates are informational and do not define normative rules.

------

## 5. Authority Relationships

The SDLC domains have an authority hierarchy.

Higher-level domains define policies that constrain lower-level domains.

The SDLC authority model is as follows:

```id="e7ay1y"
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

Governance documents define how the SDLC framework is authored and maintained.

Governance documents take precedence over all other SDLC domains.

------

### architecture precedence

Architecture documents define structural constraints on software systems.

Architecture rules take precedence over standards, guidelines, and tooling conventions.

------

### standards precedence

Standards define mandatory programming and engineering rules.

Standards take precedence over guidelines and templates.

------

### guidelines precedence

Guidelines provide recommended practices and MAY be deviated from when justified.

Guidelines do not override standards or architecture rules.

------

### documentation and tooling

Documentation and tooling domains provide supporting standards and practices.

These domains MUST remain consistent with architecture and programming standards.

------

## 7. Cross-Domain Consistency

Documents within the SDLC framework MUST remain internally consistent.

When conflicts occur between documents:

1. the higher-authority domain prevails
2. the conflicting document SHOULD be revised to restore consistency

Cross-domain references SHOULD be maintained to clarify relationships between rules.

------

## 8. Evolution of the SDLC Structure

Changes to the structure of the SDLC framework are governed by the SDLC maintenance model defined in:

- servicing_and_maintenance_strategy
- sdlc_document_standard

Structural changes SHOULD be introduced through amendment documents and consolidated periodically.

------

## 9. References

sdlc_overview
sdlc_document_standard
engineering_governance
servicing_and_maintenance_strategy

------

At this point you now have the **three foundational documents** that anchor the entire framework:

```
sdlc_overview.md
sdlc_structure.md
sdlc_document_standard.md
```

Together they define:

- what the SDLC is
- how it is organized
- how its documents are written and maintained