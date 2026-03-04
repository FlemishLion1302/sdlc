#  ID: 2026-03-03-v1.0.1
Type: AMEND
Status: Proposed
Targets: C++ Coding Standard Baseline §4 (Project Structure Requirements), §5 (Include and Dependency Discipline)
Normative impact: Strengthens structural requirements: mandates out-of-line definitions for non-template functions, adds constrained exceptions.
Enforcement: CI + review (review-enforced immediately; CI rule may be added as tooling permits)
Migration: Existing header-defined non-template functions must be moved to corresponding .cpp unless they meet the inline exception criteria below.
Superseded by:

---

## Rationale

Header-only non-template implementation leaks dependencies and implementation details into the public surface,
increasing compile times, rebuild cascades, and coupling. The canonical repository layout already assumes a
public API in include/ and implementation in src/; this delta makes that separation normative.

Templates require visible definitions and remain exempt.

---

## Change

### 1) Add new subsection after §4.7 (Header Policy): §4.9 Out-of-Line Definition Policy

Insert the following text:

#### 4.9 Out-of-Line Definition Policy

Non-template function and method definitions **SHALL** be provided out-of-line in a corresponding `.cpp`
file, not in a header.

This applies to:
- Free functions
- Non-template member functions
- Non-template static member functions
- Non-template lambdas stored as function objects when exposed through headers (the callable body must not be defined in the header unless it meets §4.9.1)

**Exception:** Template entities (function templates, class templates, variable templates, and members of
class templates) **MAY** be defined in headers.

##### 4.9.1 Inline Exception (Header-Defined Bodies)

A non-template function body **MAY** appear in a header only when all criteria are satisfied:

1. The function is explicitly marked `inline` (or is a `constexpr`/`consteval` function).
2. The body is trivially small and side-effect free:
   - No dynamic allocation
   - No logging
   - No I/O
   - No synchronization
   - No system calls
   - No exceptions thrown
3. The body does not require additional heavy includes beyond what is already required by the public
   declaration.

When in doubt, the implementation **SHALL** be moved to `.cpp`.

##### 4.9.2 Public Header Pairing

For each public header that declares non-template functions or non-template class methods, a corresponding
implementation `.cpp` **SHALL** exist under `src/<project>/` (or a subdirectory matching the namespace).

Pure type headers (types only; no non-template function bodies) **MAY** be header-only.

---

### 2) Amend §5.5 (Public Header Restrictions) by adding this paragraph at the end

Template definitions in public headers **SHOULD** minimize include footprint. Template-heavy headers
**SHOULD** avoid pulling in third-party and platform headers unless required by the public signature.
Where practical, prefer forward declarations and type-erasure boundaries to keep public headers stable.