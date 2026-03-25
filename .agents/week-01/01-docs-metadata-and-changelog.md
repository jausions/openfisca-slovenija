# Task 01 — Docs, Metadata, and Changelog

## Schedule

Week 1, Days 1–3

## Goal

Replace template-facing repository metadata with Slovenia-specific
project documentation so that the package is understandable,
installable, and releasable.

## Scope

- Rewrite `README.md` away from the fictional template country
- Update `pyproject.toml` metadata to match the Slovenia package state
- Start maintaining `CHANGELOG.md` using the OpenFisca format
  already described in `CONTRIBUTING.md`

## Dependencies

- Read `IMPLEMENTATION_PLAN.md`
- No code dependency on other week-1 tasks
- Can run in parallel with task 02 if the README does not need
  final parameter-file paths yet

## Target files

- `README.md`
- `pyproject.toml`
- `CHANGELOG.md`
- optionally `.github/PULL_REQUEST_TEMPLATE.md` only if a tiny
  clarification is needed for changelog discipline

## Implementation details

### 1. `README.md`

Replace template content with repository-specific content:

- what `openfisca-slovenia` models now
- current implementation status: payroll-first Slovenia model,
  seeded with 2026 values, time-aware by date
- package structure overview:
  - `parameters/`
  - `variables/`
  - `tests/`
  - `reforms/`
  - `situation_examples/`
- install and development instructions that match the current `pyproject.toml`
- how to run tests locally
- note that placeholder template legislation is being replaced incrementally
- point contributors to `IMPLEMENTATION_PLAN.md` and `CONTRIBUTING.md`

Avoid claiming features that are not yet implemented.

### 2. `pyproject.toml`

Review and update:

- `description`
- `authors` and/or `maintainers` if known from repository
  ownership or explicit project metadata
- `requires-python` if the project now intends to standardise on 3.11
- classifiers if needed to match supported Python versions
- URLs if any are still template-like or incomplete
- keep dependency changes minimal unless required by actual code

Do not add speculative dependencies.

### 3. `CHANGELOG.md`

Keep the existing initial entry if still historically correct, but
add or revise entries so the changelog can support the migration
from template placeholder to Slovenia payroll model.

Follow the repo rule from `CONTRIBUTING.md`:

- heading = version number + PR link
- include change type
- include impacted periods
- include impacted areas
- include explicit user-facing details
- split unrelated changes with `<!-- -->` when needed

If the repository is still pre-release, the changelog can describe
documentation and scaffolding changes clearly without pretending
that the payroll model is complete.

## Deliverables

- README accurately reflects the project
- package metadata is coherent and non-template
- changelog discipline is in place and visible

## Acceptance criteria

- `README.md` no longer describes the fictional template country
  or template reforms as real package behaviour
- `pyproject.toml` metadata is internally consistent with the repository
- `CHANGELOG.md` entries follow the OpenFisca structure from `CONTRIBUTING.md`
- no broken Markdown structure or invalid TOML syntax

## Suggested verification

- render/read `README.md`
- run a TOML parse check if available
- run any existing metadata/lint checks relevant to packaging
