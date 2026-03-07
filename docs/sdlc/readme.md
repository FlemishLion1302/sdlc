# Engineering Governance Repository

This repository is the authoritative source of engineering governance artifacts.

All engineering standards, guidelines, architectural policies, documentation governance,
and tooling governance are defined here.

Projects MUST reference a tagged version of this repository when adopting governance.

Governance artifacts are immutable and versioned.


---

# Governance Domains

Engineering governance is structured into the following domains:

## 1. Language Standards

Defines mandatory coding standards and review-enforced guidance.

- C++ Coding Standard (CI-enforced)
- C++ Engineering Guidelines (review-enforced)

Located under:
```
docs/sdlc/engineering/standards/
docs/sdlc/engineering/guidelines/
```
These define structural correctness and language-level expectations.


---

## 2. Architecture and Taxonomy

Defines system structuring rules and dependency direction.

- Taxonomy definitions
- Layering rules
- Dependency policies

Located under:
```
docs/sdlc/engineering/taxonomy/
docs/sdlc/engineering/layering/
```
These define architectural integrity.


---

## 3. Documentation Governance

Defines documentation and compatibility policy requirements.

Includes:

- Project documentation requirements
- Source code documentation standards
- API reference generation policy
- ABI stability policy
- Deprecation policy
- Versioning policy

Located under:
```
docs/sdlc/documentation/
```
These define how projects describe, publish, and evolve their contracts.


---

## 4. Tooling Governance

Defines automation structure and reproducibility expectations.

Includes:

- Tooling and automation rules
- Repository layout conventions

Located under:
```
docs/sdlc/tooling/
```
These define how projects structure and execute automation.


---

# Governance Hierarchy

Governance hierarchy and immutability rules are defined in:
```
docs/sdlc/engineering/governance/engineering_governance.md
```
Change mechanics (delta lifecycle, version precedence, rebase policy) are defined in:
```
docs/sdlc/framework/servicing_and_maintenance_strategy.md
```
Governance artifacts are immutable once tagged.


---

# Repository Structure
```
docs/sdlc/
├─ framework/
├─ engineering/
│  ├─ governance/
│  ├─ standards/
│  ├─ guidelines/
│  ├─ layering/
│  └─ taxonomy/
├─ documentation/
│  ├─ governance/
│  ├─ guidelines/
│  └─ policies/
├─ tooling/
└─ templates/
```
---

# Adoption by Projects

Projects MUST:

1. Reference a tagged version (e.g., `v1.0.1`), not `master`.
2. Declare the adopted governance version explicitly.
3. Treat governance artifacts as immutable.

---

# Versioning Model

- Tags represent governance release points.
- Governance evolves via versioned deltas.
- Rebase events consolidate baselines.
- Tags SHOULD be created after significant governance changes.
- Projects must not reference floating branches.


---

# Immutability

Governance documents are immutable once released.

Changes MUST occur through:

- A versioned delta, or
- A formal rebase event.

Informal edits to released artifacts are prohibited.
