# cpp_coding_standard

**Status:** Authoritative
**Supersedes:**

- C++ Coding Style Policy
- C++ Project Structure Policy
- Non-Syntax / Tooling Policy

------

# 1. Document Authority and Scope

## 1.1 Status

This document is the single authoritative C++ engineering standard.

It consolidates previously published C++ engineering policies into a single normative specification.

No previously locked **SHALL** requirement is weakened by consolidation.

This document operates within the SDLC engineering governance framework.

See:

```
engineering_governance
```

The engineering governance framework defines:

- authority hierarchy
- enforcement posture
- document evolution model

When questions arise regarding cross-document authority or change control, the governance framework applies.

------

## 1.2 Scope

This standard governs:

- project structure
- file organization
- naming conventions
- formatting
- language usage discipline
- tooling and enforcement

This standard does **not** govern:

- domain modeling
- business logic design
- system architecture beyond structural discipline

Those concerns are addressed in engineering guidelines and architectural documentation.

------

## 1.3 Normative Language

Normative language follows the definitions established in the SDLC document standard.

The keywords **SHALL**, **SHOULD**, and **MAY** retain their normative meaning.

- **SHALL** indicates a mandatory requirement.
- **SHOULD** indicates a strong recommendation with limited justified exceptions.
- **MAY** indicates optional behavior.

------

# 2. Governance and Enforcement

## 2.1 CI Authority

Continuous Integration (CI) is the final enforcement authority for this standard.

CI **SHALL** reject builds for:

- formatting violations
- structural rule violations
- compilation warnings
- compilation failures

Standards without enforcement degrade; therefore this standard is CI-enforced.

------

## 2.2 Tooling Hierarchy

Authoritative tooling hierarchy (highest structural authority first):

1. `.editorconfig` — indentation, encoding, line endings, whitespace
2. `.clang-format` — formatting and include handling
3. centralized build configuration — compiler settings and build behavior
4. `.gitattributes` — line-ending normalization and text handling

IDE configuration **MUST** conform to repository configuration.

IDE preferences **SHALL NOT** override repository policy.

------

## 2.3 Warnings-as-Errors

All compiler warnings **SHALL** be treated as errors.

Projects **SHALL NOT**:

- lower warning levels
- globally disable warnings
- relax treat-warnings-as-errors

Local suppression requires explicit justification and minimal scope.

Compiler configuration **SHALL**:

- use a high warning level equivalent to `/W4`
- enable standards-conforming mode (`/permissive-`)
- enable the standard preprocessor
- enable security development lifecycle checks
- compile as UTF-8

Compiler policy **SHALL** be centralized via shared build configuration.

------

## 2.4 Conformance Definition

A project conforms if and only if:

- it compiles with zero warnings
- it passes formatting checks
- it adheres to structural layout rules
- it respects include discipline rules
- it does not violate public surface containment

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

Projects that opt into C++20 **SHALL** document this elevation in project configuration.

The compiler **SHALL** operate in standards-conforming mode.

Source files **SHALL** be compiled as UTF-8.

------

## 3.3 Migration Discipline

During transition from C++17 to C++20:

- new code **SHOULD NOT** introduce C++20-only features into C++17 projects
- standard library facilities unavailable in C++17 **SHALL NOT** be used unless the project explicitly targets C++20
- version elevation **SHALL** be deliberate and repository-visible

------

## 3.4 Version Independence Constraint

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
- mixed extension conventions

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

## 4.9 Out-of-Line Definition Policy

Non-template function and method definitions **SHALL** be provided out-of-line in a corresponding `.cpp` file.

This applies to:

- free functions
- non-template member functions
- non-template static member functions

Template entities remain exempt.

------

## 4.9.1 Inline Exception (Header-Defined Bodies)

A non-template function body **MAY** appear in a header only when all criteria are satisfied:

1. the function is explicitly marked `inline` (or is `constexpr` / `consteval`)
2. the body is trivially small and side-effect free
3. the body does not require additional heavy includes

When in doubt, the implementation **SHALL** be moved to `.cpp`.

------

## 4.9.2 Public Header Pairing

For each public header declaring non-template functions or class methods, a corresponding implementation `.cpp` **SHALL** exist.

Pure type headers **MAY** remain header-only.

------

# 5. Include and Dependency Discipline

## 5.1 Self-Header-First

Each `.cpp` file **SHALL** include its corresponding header first.

------

## 5.2 Header Self-Sufficiency

Public headers **MUST** compile independently.

They **SHALL NOT** rely on:

- transitive includes
- include order
- other compilation units

------

## 5.3 Include Group Ordering

Includes **SHALL** be grouped in this order:

1. self header
2. standard library
3. third-party libraries
4. project headers

------

## 5.4 Explicit Includes

Code **SHALL NOT** rely on transitive includes.

All dependencies **MUST** be explicitly included.

------

## 5.5 Public Header Restrictions

Public headers **SHALL NOT** include:

- `detail/` headers
- platform-specific headers
- heavy implementation headers unless strictly required

Public surface **SHALL** remain minimal and stable.

------

# 6. Naming Conventions

Namespaces **SHALL** use `lower_snake_case`.

Types **SHALL** use `PascalCase`.

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

Formatting **SHALL** be machine-enforced.

Brace style: **Allman**

Indentation:

- 4 spaces
- no tabs

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
Directory.Build.targets
```

`.gitattributes` normalizes line endings.

`.gitignore` excludes `/build`.

------

# 10. Conformance and Exceptions

A project conforms only if:

- zero warnings
- successful CI
- structural compliance
- formatting compliance

CI failure blocks integration.

Exceptions **MUST** be justified, minimal, and documented.

------

# 11. Amendment and Evolution

This document is a **baseline engineering standard**.

Changes to this document **SHOULD** normally be introduced through amendment documents as defined by:

```
engineering_governance
sdlc_document_standard
```

Amendments allow incremental evolution while preserving baseline stability.

Over time, amendments may be incorporated into revised baseline editions through consolidation.

------

# 12. Decision Record (Informative)

This section records rationale and historical decisions.

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

