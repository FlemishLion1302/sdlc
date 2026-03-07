# Source Code Documentation Standard

## 1. Purpose

This document defines the minimum requirements for documenting source code
within SDLC-governed repositories.

Source documentation serves three primary purposes:

- Define the public contract of the system
- Communicate usage expectations and constraints
- Enable automated API reference generation

Documentation is part of the codebase and must evolve with it.

------

# 2. Scope

This standard applies to:

- Public headers
- Public classes
- Public functions
- Public enums
- Public templates
- Public macros (if exposed)
- Exported C interfaces

This standard does **NOT** require extensive documentation of:

- Private implementation details
- Internal helper functions
- Anonymous namespaces
- Test-only code

Over-documentation of private code is discouraged.

------

# 3. Public API Documentation Requirements

Every publicly exposed type or function MUST include documentation.

## 3.1 Summary

A one-sentence description explaining what the entity does.

The summary must describe **behavior**, not implementation.

## 3.2 Contract Requirements

Where applicable, documentation MUST define:

- Preconditions
- Postconditions
- Ownership / lifetime expectations
- Thread-safety guarantees
- Error behavior

If a rule is implicit but critical, it must be made explicit.

------

# 4. Function Documentation Requirements

Public functions MUST document:

- Purpose
- Parameters (if non-trivial)
- Return value semantics
- Error conditions
- Side effects
- Thread-safety if relevant

Do not restate obvious type information.

Document **semantics**, not syntax.

------

# 5. Class Documentation Requirements

Public classes MUST document:

- High-level purpose
- Invariants
- Ownership model
- Thread-safety guarantees
- Usage expectations

If misuse is possible, document correct usage patterns.

------

# 6. Enum Documentation

Public enums MUST document:

- What the enum represents
- Meaning of each enumerator
- Whether new enumerators may be added in future versions

Enums that are part of ABI contracts must clearly indicate stability guarantees.

------

# 7. Templates and Generic Code

Templates must document:

- Required type constraints
- Expected semantics of template parameters
- Complexity guarantees if relevant

Do not rely solely on static assertions to communicate intent.

------

# 8. Error Model Documentation

Projects MUST clearly document one of the following error models:

- Exceptions
- Error codes / status objects
- `expected`-like return types
- Mixed model (if explicitly defined)

Each public API must align with the documented error model.

Error behavior must be consistent and predictable.

------

# 9. Thread-Safety Documentation

If a component:

- Is thread-safe
- Is conditionally thread-safe
- Is not thread-safe

This must be explicitly documented.

Absence of documentation implies **NOT thread-safe**.

------

# 10. Documentation Format

Projects SHOULD use a structured documentation style compatible with
automated tooling (e.g., Doxygen-style comments for C++).

Example style:

```cpp
/**
 * Summary sentence describing the API.
 *
 * Additional context if necessary.
 */
```

Formatting details are left to the project toolchain.

The SDLC does not mandate a specific documentation generator.

------

# 11. Prohibited Practices

The following are prohibited:

- Copying implementation code into documentation
- Documenting private implementation details unnecessarily
- Writing documentation that contradicts behavior
- Allowing stale documentation after behavior changes
- Using documentation as a substitute for clear API design

------

# 12. CI and Review Expectations

Projects SHOULD:

- Treat undocumented public APIs as review findings
- Optionally fail CI for undocumented public symbols
- Require documentation updates in PRs that change public behavior

Code review must consider documentation correctness as part of acceptance.

------

# 13. Minimal Compliance Checklist

A repository is compliant if:

- All public types and functions are documented
- Error model is documented
- Thread-safety is documented
- Public contracts are explicit
- Documentation is kept in sync with behavior

Failure to document public APIs constitutes SDLC non-compliance.

------

# 14. API Usage Examples

Public APIs SHOULD include a concise usage example to illustrate correct
and idiomatic use.

Examples improve discoverability, reduce misuse, and provide immediate
context for API consumers.

## 14.1 Requirement

Each public API SHOULD provide a short usage example when the API is:

- Non-trivial
- Likely to be misused
- Part of a commonly used interface
- Introduced as a new feature

Simple or self-evident APIs may omit examples.

## 14.2 Example Length

Examples must be concise.

Guidelines:

- Maximum length: **15 lines**
- Demonstrate typical usage
- Prefer complete minimal snippets
- Avoid unnecessary scaffolding

## 14.3 Documentation Syntax

Examples should use the following pattern:

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
    handle_error(status);
}
@endcode
```

## 14.4 Placement

Examples must be associated with the API they document.

Examples should appear in the documentation block for:

- a function
- a class
- a struct
- an enum
- a template

Examples should **not** be placed at file-level when they describe a specific API.

File-level examples may be used only for:

- multi-API workflows
- tutorials
- high-level system documentation.

------

# 15. Standardized Documentation Tags

Projects using automated documentation generators (e.g., Doxygen)
must use a consistent set of documentation tags.

Standardizing tag usage ensures:

- consistent API reference output
- predictable formatting
- easier documentation review
- compatibility with automated tooling.

------

## 15.1 Preferred Comment Style

Documentation comments should use block-style comments:

```cpp
/**
 * Summary sentence describing the API.
 *
 * Additional context if necessary.
 */
```

Single-line (`///`) comments may be used for short descriptions.

------

## 15.2 Approved Tag Set

### Core Description

| Tag        | Purpose                          |
| ---------- | -------------------------------- |
| `@brief`   | Short summary of the API         |
| `@details` | Extended description when needed |

Projects may omit `@brief` if the first sentence clearly serves as the summary.

------

### Parameters and Returns

| Tag       | Purpose                        |
| --------- | ------------------------------ |
| `@param`  | Function parameter description |
| `@tparam` | Template parameter description |
| `@return` | Return value description       |

Parameter descriptions should explain **semantic meaning**, not types.

------

### Examples

| Tag            | Purpose                       |
| -------------- | ----------------------------- |
| `@par Example` | Introduces an example section |
| `@code`        | Start code block              |
| `@endcode`     | End code block                |

------

### Notes and Behavior

| Tag          | Purpose                   |
| ------------ | ------------------------- |
| `@note`      | Additional information    |
| `@warning`   | Important caveat          |
| `@attention` | Critical usage constraint |

------

### Cross References

| Tag    | Purpose                  |
| ------ | ------------------------ |
| `@see` | Related APIs             |
| `@ref` | Explicit cross-reference |

------

## 15.3 Prohibited or Discouraged Tags

Projects should avoid inconsistent tags such as:

- `@returns` (use `@return`)
- `@arg` (use `@param`)
- custom tags unless formally defined
- free-form headings in comments

Consistency takes precedence over stylistic preference.

------

## 15.4 Ordering of Documentation Sections

Documentation sections should follow this order:

1. Summary (`@brief`)
2. Extended description (`@details`)
3. Example (`@par Example`)
4. Parameters (`@param`)
5. Template parameters (`@tparam`)
6. Return value (`@return`)
7. Notes (`@note`, `@warning`)
8. Cross references (`@see`)

------

## 15.5 Review Expectations

Code review must ensure:

- approved tags are used
- examples follow the ≤15 line rule
- tags are consistently ordered
- documentation matches actual API behavior

Documentation quality is part of API correctness.

------

# 16. Canonical API Documentation Template

Projects SHOULD use the following template when documenting public APIs.

```cpp
/**
 * @brief One-sentence description of the API.
 *
 * Optional extended description explaining behavior or constraints.
 *
 * @par Example
 * @code
 * daedalus::Result<int> result = compute();
 *
 * if (!result)
 * {
 *     handle_error(result.status());
 *     return;
 * }
 *
 * std::cout << result.value() << std::endl;
 * @endcode
 *
 * @param input Description of the parameter's meaning.
 * @param flags Optional flags controlling behavior.
 *
 * @return Description of the returned value.
 *
 * @note Additional usage notes if necessary.
 *
 * @see related_api
 */
```

This template ensures:

- consistent formatting
- predictable documentation output
- concise usage examples
- alignment with SDLC documentation standards.

