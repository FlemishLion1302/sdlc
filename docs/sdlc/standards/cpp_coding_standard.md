# C++ Coding Standard

**Status:** Authoritative
**Supersedes:**

- C++ Coding Style Policy
- C++ Project Structure Policy
- Non-Syntax / Tooling Policy

------

# 1. Document Authority and Scope

## 1.1 Status

This document is the single authoritative C++ engineering standard.
It consolidates previously published policies without altering their normative meaning.

No previously locked **SHALL** requirement is weakened by consolidation.

### Governance Framework Reference

This document operates within the repository’s Engineering Governance Framework.

See:

```
docs/engineering/engineering_governance.md
```

The Governance Framework defines authority hierarchy, enforcement posture, and document immutability requirements across all engineering policy artifacts.

In case of ambiguity regarding cross-document authority or change control, the Governance Framework applies.

------

## 1.2 Scope

This standard governs:

- Project structure
- File organization
- Naming conventions
- Formatting
- Language usage discipline
- Tooling and enforcement

It does not govern:

- Domain modeling
- Business logic design
- Architectural decisions beyond structural discipline

------

## 1.3 Normative Language

The keywords **SHALL**, **MUST**, **SHOULD**, and **MAY** retain their previously defined meanings.

- **SHALL** indicates a mandatory requirement.
- **SHOULD** indicates strong recommendation with limited justified exceptions.
- **MAY** indicates optional behavior.

------

# 2. Governance and Enforcement

## 2.1 CI Authority

CI is the final enforcement authority for this standard.

CI **SHALL** reject builds for:

- Formatting violations
- Structural rule violations
- Compilation warnings
- Compilation failures

Standards without enforcement degrade. This standard is CI-enforced.

------

## 2.2 Tooling Hierarchy

Authoritative tooling hierarchy (highest structural authority first):

1. `.editorconfig` — indentation, encoding, line endings, whitespace
2. `.clang-format` — formatting and include handling
3. Centralized MSBuild properties — compiler settings and build behavior
4. `.gitattributes` — line-ending normalization and text handling

IDE configuration **MUST** conform to repository configuration.
IDE preferences **SHALL NOT** override repository policy.

------

## 2.3 Warnings-as-Errors

All compiler warnings **SHALL** be treated as errors.

Projects **SHALL NOT**:

- Lower warning levels
- Globally disable warnings
- Relax treat-warnings-as-errors

Local suppression requires explicit justification and minimal scope.

Compiler configuration **SHALL**:

- Use a high warning level equivalent to `/W4`
- Enable standards-conforming mode (`/permissive-`)
- Enable the standard preprocessor
- Enable security development lifecycle checks
- Compile as UTF-8

Compiler policy **SHALL** be centralized via shared MSBuild property files.

------

## 2.4 Conformance Definition

A project conforms if and only if:

- It compiles with zero warnings
- It passes formatting checks
- It adheres to structural layout rules
- It respects include discipline rules
- It does not violate public surface containment

Conformance is determined by CI.

------

# 3. Language Version Policy

## 3.1 Minimum Standard

Minimum supported language standard: **C++17**.

All projects **SHALL** compile in standards-conforming mode.

------

## 3.2 Default Standard

Default language standard: **C++17**, unless explicitly elevated.

Use of **C++20** (or newer) features:

- **SHALL** be explicitly declared at the project level.
- **SHALL NOT** be introduced implicitly into C++17 projects.
- **SHOULD** be justified during migration or architectural review.

Projects that opt into C++20 **SHALL** document this elevation in their project configuration.

The compiler **SHALL** operate in standards-conforming mode.
Source files **SHALL** be compiled as UTF-8.

------

## 3.3 Migration Discipline

During transition from C++17 to C++20:

- New code **SHOULD NOT** introduce C++20-only features into C++17 projects.
- Standard library facilities unavailable in C++17 **SHALL NOT** be used unless the project explicitly targets C++20.
- Version elevation **SHALL** be deliberate and repository-visible.

------

## 3.4 Version Independence Constraint

This standard is minimally tied to language version.
Structural and formatting rules **SHALL NOT** depend on optional language features.

------

# 4. Project Structure Requirements

## 4.1 Repository Layout

Canonical layout:

```
/include/<project_name>/
/src/<project_name>/
/src/<project_name>/detail/
/build/
```

No alternate structural patterns are permitted.

Tests **SHALL** reside outside the public include tree.

------

## 4.2 Public Header Placement

Public headers **SHALL** reside under:

```
include/<project_name>/
```

This directory defines the entire public API surface.

Public headers **MUST** compile independently.

------

## 4.3 Private Header Placement

Private headers **SHALL** reside under:

```
src/<project_name>/detail/
```

Private headers **SHALL NOT** be included from public headers.

------

## 4.4 Namespace–Directory Alignment

Namespace hierarchy **MUST** mirror directory hierarchy.

Example:

```
include/foo/bar/baz.h → namespace foo::bar
```

This rule is mandatory and CI-enforced.

------

## 4.5 Build Artifact Containment

All generated artifacts **SHALL** reside under:

```
/build
```

No `obj/`, `bin/`, or intermediate artifacts may exist elsewhere.

------

## 4.6 File Extensions

- Headers: `.h`
- Sources: `.cpp`

Rejected alternatives:

- `.hpp`
- `.cc`
- Mixed extension conventions

------

## 4.7 Header Policy

Headers **SHALL** use:

```
#pragma once
```

Traditional include guards **SHALL NOT** be combined with `#pragma once`.

Each `.cpp` file **SHALL** include its corresponding header first.

------

## 4.8 Directory and File Naming

Directories **SHALL** use `lower_snake_case`.

Source and header filenames **SHALL** use `lower_snake_case`.

------

# 4.9 Out-of-Line Definition Policy

Non-template function and method definitions **SHALL** be provided out-of-line in a corresponding `.cpp` file, not in a header.

This applies to:

- Free functions
- Non-template member functions
- Non-template static member functions
- Non-template lambdas stored as function objects when exposed through headers

Template entities remain exempt.

------

## 4.9.1 Inline Exception (Header-Defined Bodies)

A non-template function body **MAY** appear in a header only when all criteria are satisfied:

1. The function is explicitly marked `inline` (or is a `constexpr` / `consteval` function).
2. The body is trivially small and side-effect free:
   - No dynamic allocation
   - No logging
   - No I/O
   - No synchronization
   - No system calls
   - No exceptions thrown
3. The body does not require additional heavy includes beyond what is already required by the public declaration.

When in doubt, the implementation **SHALL** be moved to `.cpp`.

------

## 4.9.2 Public Header Pairing

For each public header that declares non-template functions or non-template class methods, a corresponding implementation `.cpp` **SHALL** exist under `src/<project_name>/` (or a subdirectory matching the namespace).

Pure type headers **MAY** be header-only.

------

# 5. Include and Dependency Discipline

## 5.1 Self-Header-First

Each `.cpp` file **SHALL** include its corresponding header first.

------

## 5.2 Header Self-Sufficiency

Public headers **MUST** compile independently.

They **SHALL NOT** rely on:

- Transitive includes
- Include order
- Other compilation units

------

## 5.3 Include Group Ordering

Includes **SHALL** be grouped in this order:

1. Self header
2. Standard library
3. Third-party libraries
4. Project headers

------

## 5.4 Explicit Includes

Code **SHALL NOT** rely on transitive includes.
All dependencies **MUST** be explicitly included.

------

## 5.5 Public Header Restrictions

Public headers **SHALL NOT** include:

- `detail/` headers
- Platform-specific headers
- Heavy implementation headers unless strictly required

Public surface **SHALL** remain minimal and stable.

Template definitions in public headers **SHOULD** minimize include footprint.
Template-heavy headers **SHOULD** avoid pulling in third-party and platform headers unless required by the public signature.

Where practical, prefer forward declarations and type-erasure boundaries to keep public headers stable.

------

# 6. Naming Conventions

Namespaces **SHALL** use `lower_snake_case`.

Classes, structs, enums, and type aliases **SHALL** use `PascalCase`.

Functions and variables **SHALL** use `lower_snake_case`.

Private members **SHALL** use `lower_snake_case_`.

Enumerations **SHALL** use `enum class`.

Enumerators **SHALL** use `PascalCase`.

Traits use `is_xxx`, `is_xxx_v`, and `xxx_t`.

Factories use `make_xxx()`.

Constants use `inline constexpr` with `lower_snake_case`.

Macros use `ALL_UPPER_CASE`.

------

# 7. Language Usage Rules

Observational methods **SHALL** be marked `const`.

Functions that do not throw **SHALL** be marked `noexcept`.

`[[nodiscard]]` **SHALL** be used where ignoring a return value is likely a defect.

`constexpr` **SHALL** be used when natural and beneficial.

Destructors **SHALL NOT** throw.

------

# 8. Formatting Rules

Formatting **SHALL** be machine-enforced via `.clang-format` and `.editorconfig`.

Brace style: **Allman**

Indentation:

- 4 spaces
- No tabs

Line endings: **LF**

Encoding: **UTF-8 without BOM**

Maximum line width **SHOULD NOT** exceed **120 characters**.

------

# 9. Tooling Configuration

`.editorconfig` governs indentation, encoding, whitespace.

`.clang-format` governs formatting and include ordering.

Build configuration is centralized via:

```
Directory.Build.props
Directory.Build.Targets
```

`.gitattributes` normalizes line endings.

`.gitignore` excludes `/build`.

------

# 10. Conformance and Exceptions

A project conforms only if:

- Zero warnings
- Successful CI
- Structural compliance
- Formatting compliance

CI failure blocks integration.

Exceptions **MUST** be justified, minimal, and documented.

------

# 11. Amendment and Evolution

Future changes **SHALL**:

- Preserve deterministic formatting
- Preserve surface containment
- Preserve CI enforceability
- Avoid IDE lock-in
- Favor mainstream compatibility

Locked decisions include:

- Allman brace style
- 4-space indentation
- LF line endings
- UTF-8 encoding
- `.h` / `.cpp` only
- Warnings as errors
- Namespace-directory alignment

------

# 12. Decision Record (Informative)

This section preserves rationale, trade-offs, historical context, and locked decisions.

It **SHALL NOT** introduce new requirements.

------

# Appendix A — Canonical C++ Example (Informative)

*(Appendix retained unchanged from the baseline)*

### A.1 Public Header Example

```cpp
#pragma once

#include <cstddef>
#include <string>

namespace foo
{

    enum class WidgetState
    {
        Idle,
        Active,
        Failed
    };

    struct WidgetConfig
    {
        std::size_t max_items = 0;
        bool enable_feature = false;
    };

    class Widget
    {
    public:
        explicit Widget(WidgetConfig config);

        [[nodiscard]] WidgetState state() const noexcept;
        [[nodiscard]] std::size_t item_count() const noexcept;

        void activate() noexcept;

        [[nodiscard]] bool try_add_item(std::string item);

    private:
        WidgetConfig config_;
        WidgetState state_ = WidgetState::Idle;
        std::size_t item_count_ = 0;
    };

}
```

------

### A.2 Private Header Example

```cpp
#pragma once

#include <string>
#include <vector>

namespace foo::detail
{

    class widget_impl
    {
    public:
        explicit widget_impl(bool enable_feature);

        void activate() noexcept;

        [[nodiscard]] bool try_add_item(std::string item);
        [[nodiscard]] std::size_t item_count() const noexcept;

    private:
        bool enable_feature_ = false;
        std::vector<std::string> items_;
    };

}
```

------

### A.3 Source Example

```cpp
#include <foo/widget.h>

#include <utility>

#include "foo/detail/widget_impl.h"

namespace
{

    [[nodiscard]] bool is_valid_item(const std::string& item) noexcept
    {
        return !item.empty();
    }

}

namespace foo
{

    Widget::Widget(WidgetConfig config)
        : config_(config)
    {
    }

    [[nodiscard]] WidgetState Widget::state() const noexcept
    {
        return state_;
    }

    [[nodiscard]] std::size_t Widget::item_count() const noexcept
    {
        return item_count_;
    }

    void Widget::activate() noexcept
    {
        state_ = WidgetState::Active;
    }

    [[nodiscard]] bool Widget::try_add_item(std::string item)
    {
        if (!is_valid_item(item))
        {
            state_ = WidgetState::Failed;
            return false;
        }

        ++item_count_;
        return true;
    }

}
```

