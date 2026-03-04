# Source Code Documentation Standard

## 1. Purpose

This document defines the minimum requirements for documenting source code
within SDLC-governed repositories.

Source documentation serves three primary purposes:

- Define the public contract of the system
- Communicate usage expectations and constraints
- Enable automated API reference generation

Documentation is part of the codebase and must evolve with it.


---

## 2. Scope

This standard applies to:

- Public headers
- Public classes
- Public functions
- Public enums
- Public templates
- Public macros (if exposed)
- Exported C interfaces

This standard does NOT require extensive documentation of:

- Private implementation details
- Internal helper functions
- Anonymous namespaces
- Test-only code

Over-documentation of private code is discouraged.


---

## 3. Public API Documentation Requirements

Every publicly exposed type or function MUST include:

### 3.1 Summary

A one-sentence description explaining what the entity does.

The summary must describe behavior, not implementation.


### 3.2 Contract Requirements

Where applicable, documentation MUST define:

- Preconditions
- Postconditions
- Ownership / lifetime expectations
- Thread-safety guarantees
- Error behavior

If a rule is implicit but critical, it must be made explicit.


---

## 4. Function Documentation Requirements

Public functions MUST document:

- Purpose
- Parameters (if non-trivial)
- Return value semantics
- Error conditions
- Side effects
- Thread-safety if relevant

Do not restate obvious type information.

Document semantics, not syntax.


---

## 5. Class Documentation Requirements

Public classes MUST document:

- High-level purpose
- Invariants
- Ownership model
- Thread-safety guarantees
- Usage expectations

If misuse is possible, document correct usage patterns.


---

## 6. Enum Documentation

Public enums MUST document:

- What the enum represents
- Meaning of each enumerator
- Whether new enumerators may be added in future versions

Enums that are part of ABI contracts must clearly indicate stability guarantees.


---

## 7. Templates and Generic Code

Templates must document:

- Required type constraints
- Expected semantics of template parameters
- Complexity guarantees if relevant

Do not rely solely on static assertions to communicate intent.


---

## 8. Error Model Documentation

Projects MUST clearly document one of the following error models:

- Exceptions
- Error codes / status objects
- `expected`-like return types
- Mixed model (if explicitly defined)

Each public API must align with the documented error model.

Error behavior must be consistent and predictable.


---

## 9. Thread-Safety Documentation

If a component:

- Is thread-safe
- Is conditionally thread-safe
- Is not thread-safe

This must be explicitly documented.

Absence of documentation implies NOT thread-safe.


---

## 10. Documentation Format

Projects SHOULD use a structured documentation style compatible with
automated tooling (e.g., Doxygen-style comments for C++).

Example style:

- Brief summary line
- Parameter descriptions
- Return description
- Contract notes

Formatting details are left to the project toolchain.

The SDLC does not mandate a specific documentation generator.


---

## 11. Prohibited Practices

The following are prohibited:

- Copying implementation code into documentation
- Documenting private implementation details unnecessarily
- Writing documentation that contradicts behavior
- Allowing stale documentation after behavior changes
- Using documentation as a substitute for clear API design


---

## 12. CI and Review Expectations

Projects SHOULD:

- Treat undocumented public APIs as review findings
- Optionally fail CI for undocumented public symbols
- Require documentation updates in PRs that change public behavior

Code review must consider documentation correctness as part of acceptance.


---

## 13. Minimal Compliance Checklist

A repository is compliant if:

- All public types and functions are documented
- Error model is documented
- Thread-safety is documented
- Public contracts are explicit
- Documentation is kept in sync with behavior

Failure to document public APIs constitutes SDLC non-compliance.

Good idea. Right now your **source code documentation standard** says “use Doxygen-style comments” but doesn’t **standardize the tag set**. That leads to drift (`@brief` vs summary sentences, `@return` vs `@returns`, etc.).

You want a **controlled tag vocabulary**.

This should be added as a **new section in `source_code_documentation.md`**, something like **“Documentation Tag Standardization”**.

Below is a pragmatic amendment that fits your governance style.

---
## 14. API Usage Examples

Public APIs SHOULD include a concise usage example to illustrate correct
and idiomatic use.

Examples improve discoverability, reduce misuse, and provide immediate
context for API consumers.

### 14.1 Requirement

Each public API SHOULD provide a short usage example when the API is:

- Non-trivial
- Likely to be misused
- Part of a commonly used interface
- Introduced as a new feature

Simple or self-evident APIs may omit examples.

### 14.2 Example Format

Examples must be concise and focused.

Guidelines:

- Maximum length: **15 lines**
- Demonstrate **typical usage**, not exhaustive behavior
- Prefer **complete minimal snippets** over fragments
- Avoid unnecessary scaffolding

Examples must compile logically even if not intended as full programs.

### 14.3 Documentation Syntax

Examples should use the following documentation pattern:
```
@par Example
@code
// minimal usage example
@endcode
```
Example:
```
@par Example
@code
daedalus::Status status = do_operation();

if (!status.ok())
{
std::cerr << status.message() << std::endl;
}
@endcode
```
### 14.4 Placement

Usage examples should appear directly after the API description and
before parameter documentation when possible.

Recommended ordering:

1. Summary
2. Example
3. Parameters
4. Return value
5. Notes / constraints

### 14.5 Quality Expectations

Examples must:

- Demonstrate correct usage
- Reflect the current API
- Avoid deprecated interfaces
- Follow project coding standards

Outdated examples must be updated alongside API changes.

### 14.6 CI and Review Expectations

Projects SHOULD treat missing or misleading examples as documentation
review findings.

Code reviewers should verify that examples remain accurate when API
behavior changes.

---
## 15. Standardized Documentation Tags

Projects using automated documentation generators (e.g., Doxygen)
must use a consistent set of documentation tags.

Standardizing tag usage ensures:

- consistent API reference output
- predictable formatting
- easier documentation review
- compatibility with automated tooling

Projects should avoid inventing custom documentation patterns
when a standard tag exists.

---

## 15.1 Preferred Comment Style

Documentation comments should use the block style:
```
/**
- Summary sentence describing the API.
- 
- Additional context if necessary.
  */
```
Single-line forms (`///`) may be used for short descriptions,
but block comments are preferred for public APIs.

---

## 15.2 Approved Tag Set

The following tags are the standard set for SDLC-governed projects.

### Core Description

| Tag | Purpose |
|----|----|
| `@brief` | Short summary of the API |
| `@details` | Extended description when needed |

Projects may omit `@brief` if the first sentence clearly serves as the summary.

---

### Parameters and Returns

| Tag | Purpose |
|----|----|
| `@param` | Function parameter description |
| `@tparam` | Template parameter description |
| `@return` | Return value description |

Parameter descriptions should describe **semantic meaning**, not types.

---

### Examples

| Tag | Purpose |
|----|----|
| `@par Example` | Introduces an example section |
| `@code` | Start code block |
| `@endcode` | End code block |

Example:
```
@par Example
@code
daedalus::Status status = do_operation();
if (!status.ok())
{
handle_error(status);
}
@endcode
```
---

### Notes and Behavior

| Tag | Purpose |
|----|----|
| `@note` | Additional information |
| `@warning` | Important caveat or constraint |
| `@attention` | Critical usage requirement |

These tags should be used sparingly and only when behavior may surprise users.


---

### Cross References

| Tag | Purpose |
|----|----|
| `@see` | Related APIs |
| `@ref` | Explicit cross reference |

Cross references help improve navigation in generated documentation.

---

## 15.3 Prohibited or Discouraged Tags

Projects should avoid inconsistent or redundant tags such as:

- `@returns` (use `@return`)
- `@arg` (use `@param`)
- custom tags unless formally defined
- free-form headings inside comments

Consistency takes precedence over stylistic preference.

---

## 15.4 Ordering of Documentation Sections

Documentation sections should follow this order when applicable:

1. Summary (`@brief`)
2. Extended description (`@details`)
3. Example (`@par Example`)
4. Parameters (`@param`)
5. Template parameters (`@tparam`)
6. Return value (`@return`)
7. Notes (`@note`, `@warning`)
8. Cross references (`@see`)

This ordering ensures consistent reference documentation.

---

## 15.5 Review Expectations

Code review must ensure:

- approved tags are used
- examples follow the <15 line rule
- tags are consistently ordered
- documentation matches actual API behavior

Documentation quality is part of API correctness.