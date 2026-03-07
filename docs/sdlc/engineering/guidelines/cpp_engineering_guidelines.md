# cpp_engineering_guidelines

# 1. Purpose

This document provides engineering guidance that complements the C++ Coding Standard.

While the Coding Standard defines deterministic, CI-enforced structural rules, these Guidelines provide recommended engineering practices intended to improve system maintainability, clarity, and operational robustness.

The Guidelines focus on cross-cutting engineering strategy rather than formatting, repository structure, or mechanical coding rules.

------

# 2. Scope

These Guidelines govern engineering strategy for SDLC-governed C++ projects, including:

- command-line processing
- error handling models
- logging and observability
- resource ownership patterns
- configuration layering
- dependency boundaries
- concurrency discipline
- testing strategy

These Guidelines do **not** govern:

- formatting
- naming conventions
- repository structure
- CI enforcement
- compiler configuration

Those concerns remain exclusively governed by the C++ Coding Standard.

------

# 3. Status

This document defines the baseline edition of the C++ Engineering Guidelines.

Guidance may evolve through amendment documents as defined by:

- `engineering_governance`
- `sdlc_document_standard`

Amendments refine or extend this document without modifying the baseline directly.

Over time, amendments may be consolidated into revised baseline editions.

This document does **not** alter or weaken any requirement defined by the C++ Coding Standard.

------

# 4. Normative Language

Normative language follows the definitions established by the SDLC document standard.

This document primarily uses:

- **SHOULD** — strong recommendation
- **RECOMMENDED** — preferred practice
- **AVOID** — strongly discouraged
- **MAY** — optional

The keyword **SHALL** is used sparingly because these Guidelines are review-enforced rather than CI-enforced.

------

# 5. Section Number Stability

Section numbering follows the stability rules defined by the SDLC document standard.

Amendments **SHOULD** extend existing sections using subordinate numbering where practical rather than renumbering existing sections.

------

# 6. Command-Line Processing

## 6.1 Library Usage

Projects **SHOULD** use a dedicated argument parsing library rather than manually parsing `argv`.

Manual parsing is acceptable only for trivial tools.

------

## 6.2 CLI Configuration Object Pattern

Command-line processing **SHOULD**:

1. parse input arguments
2. validate inputs
3. produce a configuration object
4. transfer control to application logic

The `main()` function **SHOULD NOT** contain business logic.

------

## 6.3 Deterministic Behavior

CLI options **SHOULD**:

- have deterministic precedence rules
- provide clear help output
- fail fast on invalid input

------

## 6.4 Application Bootstrap Structure

Executable applications **SHOULD** follow a deterministic bootstrap sequence:

1. parse CLI arguments
2. validate inputs
3. construct a configuration object
4. initialize infrastructure
5. invoke the application entry point
6. return a well-defined exit code

The `main()` function **SHOULD** contain orchestration only and **SHOULD NOT** contain business logic.

------

## 6.5 Configuration Object Semantics

CLI parsing **SHOULD** produce a strongly typed configuration object.

The configuration object:

- **SHOULD** represent validated inputs
- **SHOULD** be immutable after construction
- **SHOULD NOT** expose raw parsing artifacts

Business logic **SHOULD** depend on the configuration object rather than `argc` / `argv`.

------

## 6.6 Command Dispatch Pattern

For tools supporting subcommands:

- a dispatch layer **SHOULD** map subcommands to discrete handlers
- each handler **SHOULD** receive a typed configuration specific to the command
- command registration **SHOULD** be centralized

Avoid large conditional chains in `main()`.

------

## 6.7 Deterministic Precedence Rules

When CLI options interact with configuration files or environment variables, precedence **SHOULD** be deterministic and documented.

Recommended precedence:

1. built-in defaults
2. configuration files
3. environment variables
4. command-line arguments

------

## 6.8 Exit Code Discipline

Applications **SHOULD** define explicit exit code semantics.

Distinct failure categories **MAY** use distinct exit codes.

Exit codes **SHOULD NOT** be implicitly derived from unrelated error values.

------

## 6.9 Error Integration

The CLI layer **SHOULD** integrate with the project’s declared error model.

Exception-based projects **SHOULD** translate unhandled exceptions into deterministic exit codes.

Result-based projects **SHOULD** propagate errors to a single exit-point translator.

------

## 6.10 Testability

CLI parsing logic **SHOULD** be testable independently from process startup.

Parsing logic is **RECOMMENDED** to exist outside `main()`.

------

# 7. Error Handling Strategy

## 7.1 Explicit Error Model

Each project **SHOULD** explicitly define and document its primary error model.

New modules **SHOULD** conform to the declared model.

Cross-module boundaries **SHOULD NOT** mix exception-based and result-based signaling unless explicitly justified.

If no model is declared, result-based error handling is **RECOMMENDED**.

------

## 7.2 Exceptions

If exceptions are used:

- use for exceptional conditions
- prefer strong or basic exception safety
- avoid throwing across ABI boundaries

Destructors **SHOULD NOT** throw.

------

## 7.3 Result-Based Patterns

If using result-based error handling:

- prefer expressive result types
- provide diagnostic context
- avoid silent failure

------

## 7.4 Error Domain Design

Projects **SHOULD** define:

- clear error categories
- stable error semantics
- consistent mapping between internal and external errors

------

# 8. Logging and Observability

## 8.1 Abstraction

Logging **SHOULD** be abstracted behind an interface.

Core modules **SHOULD NOT** depend directly on a concrete logging framework.

------

## 8.2 Severity Model

Projects **SHOULD** define a consistent severity taxonomy:

- trace
- debug
- info
- warning
- error
- critical

------

## 8.3 Structured Logging

Structured logging is **RECOMMENDED** over free-form string concatenation.

------

## 8.4 Logging Discipline

Avoid:

- logging inside tight performance loops
- logging as a substitute for error propagation
- global singleton loggers

------

# 9. Resource Ownership and Lifetime

## 9.1 Explicit Ownership

Ownership semantics **SHOULD** be explicit and visible in APIs.

Use:

- `std::unique_ptr` for exclusive ownership
- `std::shared_ptr` only when required
- raw pointers or references for non-owning access

------

## 9.2 RAII

Resource lifetime **SHOULD** follow RAII principles.

Avoid separate `init()` / `shutdown()` patterns where RAII can enforce correctness.

------

## 9.3 Lifetime Boundaries

Module boundaries **SHOULD** define:

- ownership transfer rules
- destruction responsibilities
- thread-affinity constraints

------

# 10. Configuration Management

## 10.1 Layered Configuration

Configuration **SHOULD** support deterministic layering:

1. built-in defaults
2. configuration files
3. environment variables
4. command-line overrides

------

## 10.2 Immutability

Configuration objects **SHOULD** be immutable after construction.

Mutable global configuration is discouraged.

------

# 11. Dependency Management

## 11.1 Abstraction Boundaries

Modules **SHOULD** depend on abstractions rather than concrete implementations.

------

## 11.2 External Dependencies

External dependencies **SHOULD**:

- be version-pinned
- be centrally declared
- avoid leaking into public headers unnecessarily

------

## 11.3 Transitive Coupling

Avoid exposing third-party types in public APIs unless required.

------

# 12. Concurrency Strategy

## 12.1 Explicit Threading Model

Projects **SHOULD** document:

- thread ownership
- synchronization strategy
- shutdown behavior

------

## 12.2 Synchronization Discipline

Prefer:

- scoped locking
- clear lock ordering
- minimal lock scope

------

# 13. Testing Strategy

## 13.1 Determinism

Tests **SHOULD** be:

- deterministic
- independent
- free of timing-based flakiness

------

## 13.2 Test Isolation

Tests **SHOULD NOT** depend on:

- shared global mutable state
- external services without isolation or mocking

------

# 14. Evolution and Amendment

These Guidelines are expected to evolve more frequently than the Coding Standard.

Changes **SHOULD** normally be introduced through amendment documents as defined by the SDLC document standard.

Amendments **SHOULD**:

- include rationale
- preserve conceptual clarity
- avoid contradicting Coding Standard requirements
- maintain compatibility with existing guidance where practical

Promotion of guidance to the Coding Standard requires a formal revision under the authority model defined in `engineering_governance`.

------

# 15. Decision Record (Informative)

This section documents rationale, trade-offs, and architectural intent.

It **SHALL NOT** introduce new requirements.

------

## 15.1 Guideline vs Standard Separation

Engineering strategy is separated from structural enforcement.

The Coding Standard governs deterministic structure and formatting.

The Guidelines govern cross-cutting engineering decisions.

------

## 15.2 Error Model Explicitness

Projects should explicitly define their primary error model.

Implicit error semantics create inconsistency across modules.

------

## 15.3 Logging Abstraction

Core modules should not depend directly on concrete logging frameworks.

------

## 15.4 Ownership Explicitness

Ownership semantics should be visible in APIs.

------

## 15.5 Layered Configuration

Configuration layering should be deterministic and documented.

------

# Appendix A — Guideline Philosophy (Informative)

These Guidelines:

- promote architectural clarity
- encourage deterministic behavior
- avoid over-engineering
- preserve portability and maintainability
- encourage explicitness over convention

They intentionally avoid prescribing specific frameworks or vendors.

------

# Appendix B — Domain-Oriented Project Grouping (Informative)

Preferred repository grouping guidance for large solutions.

Example:

```
/<repo_root>/
    <solution_name>.sln
    Directory.Build.props
    Directory.Build.targets

    /projects/
        /core/
            /foundation/
            /logging/

        /runtime/
            /platform/
            /platform_windows/
            /platform_posix/

        /services/
            /telemetry/

        /product/
            /tool_1/
            /tool_2/

    /tests/
        /foundation_tests/
        /platform_tests/
```

