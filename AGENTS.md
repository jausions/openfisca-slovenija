# AGENTS.md

This file is a working guide for coding agents operating in `openfisca-slovenia`.

## 1. Repository purpose

This repository is building an OpenFisca country package for Slovenia.

Current direction:

- focus first on a **monthly employment payroll model**
- keep the model **time-aware** using dated legislative parameters
- use **2026** as the first seeded dataset for implementation and validation
- progressively replace the current **template placeholder** legislation
  with real Slovenia-specific logic

Do not treat 2026 as the permanent target year. The package must remain date-driven.

## 2. Source-of-truth order

When working in this repository, follow these documents in this order:

1. `IMPLEMENTATION_PLAN.md`
2. `CONTRIBUTING.md`
3. `.agents/README.md`
4. the relevant file in `.agents/week-01/` or `.agents/week-02/`
5. `CHANGELOG.md` for release/history expectations

If two sources seem inconsistent:

- prefer the higher-ranked source
- keep the change minimal
- note the inconsistency in your handoff/summary

## 3. Dispatch workflow

Use the task pack under `.agents/` for operational work.

Current execution order:

### Week 1
1. `.agents/week-01/01-docs-metadata-and-changelog.md`
2. `.agents/week-01/02-core-parameters-2026-payroll.md`
3. `.agents/week-01/03-core-payroll-variables.md`
4. `.agents/week-01/04-core-payroll-tests-and-payslip-check.md`

### Week 2
5. `.agents/week-02/05-allowances-secondary-employer-and-regres.md`
6. `.agents/week-02/06-examples-and-template-cleanup.md`

Default rule: follow the task order unless a task explicitly allows parallel work.

## 4. Coding and naming rules

### 4.1 Line length

Aim to keep line length under **90 characters**, and prefer staying under **80**
when it keeps the text readable. This is a guideline, not a strict rule.

**Exceptions are acceptable for:**

- tables and data structures that cannot be shortened without harming readability
- URLs and long link destinations
- code fences and command examples where breaking lines would confuse meaning
- long Slovenian legal names or formal references

**Apply the guideline to:**

- ordinary prose paragraphs and list items
- headings and metadata
- inline comments and docstrings

When a structure cannot reasonably fit the limit, rewrite the surrounding text
instead of forcing an awkward break. Readability and clarity take priority over
a hard line-length limit.

### 4.2 Language split

Follow OpenFisca country-package naming guidance already adopted in the plan:

- **Slovenian** for domain variable names
- **Slovenian preferred** for labels and domain terms
- **English** for Python infrastructure such as module names, class names,
  function names, and general repository/documentation structure

Examples:

- good variable name: `bruto_placa`
- good module name: `dohodnina.py`
- avoid inventing opaque internal abbreviations

### 4.3 English maintenance comments are mandatory

Whenever Slovenian appears in code, add an English explanation on the same line
or immediately above.

This applies to:

- variable names and parameter keys at definition and first use in a section
- entity or attribute names
- Slovenian legal acronyms such as `PIZ`, `DO`, `OZP`
- formulas whose intent is not obvious from pure arithmetic
- docstrings that use Slovenian terms

Example:

```python
# Compute bruto_placa (gross salary before deductions)
bruto_placa = pogodbeno_dogovorjena_placa
```

### 4.4 Naming preferences

- prefer domain-first names such as `dohodnina_osnova`
- when disambiguation is needed, use domain-first prefixes
- avoid internal abbreviations unless they are standard Slovenian legal acronyms
- keep public names stable once introduced

## 5. Time-aware legislative modeling

OpenFisca already supports values that change over time. Use that feature directly.

Rules:

- parameters must use dated `values:` entries with ISO dates
- model by **effective date**, not by "year files"
- do not encode a specific year in file names when the concept itself is time-dependent
- append new dated values when legislation changes instead of hardcoding
  conditions into formulas
- if a value is known before 2026, encode it from its real effective date when relevant

Important example:

- `minimalna_placa` must be modeled by effective date, even if in
  practice it is often updated annually

## 6. Parameter and formula design

- keep one legal concept per parameter file when practical
- keep formulas composable and testable
- prefer stable domain paths under `openfisca_slovenia/parameters/`
- prefer creating new Slovenia-specific variable modules rather than
  extending placeholder template modules when the placeholder structure
  no longer fits
- do not guess legislation when a rule is unclear; document the uncertainty instead

## 7. Testing expectations

Testing is part of the implementation, not a follow-up task.

Minimum expectations:

- add YAML tests for each isolated rule
- add integration-style payslip scenarios for complete monthly cases
- keep explicit simulation dates in tests
- freeze validated calculations as regression tests once confirmed
- record at least one manual payslip baseline when implementing a new major payroll slice

Important coverage themes from the plan:

- SSC component rates
- `OZP` flat behaviour
- PIT base flooring at zero
- PIT bracket edges
- allowance eligibility
- secondary-employer 25% withholding
- meal allowance caps and boundaries
- regres proration and exemption boundaries

## 8. Changelog and versioning discipline

Follow `CONTRIBUTING.md` and maintain `CHANGELOG.md` for every merged change.

Use SemVer:

- **Patch**: fix or improve an existing calculation
- **Minor**: add a new variable or user-facing feature
- **Major**: rename or remove a public variable/API

Each changelog entry should include:

- version number and PR link in the heading
- change type
- impacted periods with exact dates where relevant
- impacted areas as repository-relative model paths
- explicit user-facing details

Treat changelog updates as part of the definition of done.

## 9. Template cleanup and migration cautions

This repository still contains placeholder template content.

Therefore:

- do not assume existing variables, parameters, tests, reforms, or
  examples are Slovenia-valid
- remove placeholder content only when the replacement exists or when
  the cleanup task explicitly allows removal
- avoid leaving dangling imports or references to removed template files
- keep repository narrative aligned across `README.md`, examples,
  tests, parameters, variables, and reforms

## 10. Handoff expectations for agents

After completing a task, report:

- files created and edited
- tests run
- unresolved legal or data questions
- whether `CHANGELOG.md` was updated, and if not, whether it should be
  updated in the integrating PR
- any placeholder/template artifacts intentionally kept for a later cleanup step

## 11. Useful repository anchors

- package root: `openfisca_slovenia/`
- parameters: `openfisca_slovenia/parameters/`
- variables: `openfisca_slovenia/variables/`
- tests: `openfisca_slovenia/tests/`
- reforms: `openfisca_slovenia/reforms/`
- examples: `openfisca_slovenia/situation_examples/`
- main planning docs: `IMPLEMENTATION_PLAN.md`, `CONTRIBUTING.md`, `CHANGELOG.md`

## 12. Default agent behavior

Unless a task says otherwise:

- make the smallest coherent change that moves the implementation forward
- preserve public names and file structure where stability matters
- prefer explicitness over cleverness
- prefer documented uncertainty over invented legislation
- keep code and tests readable for non-Slovenian maintainers

