# Coding Agent Dispatch Pack

This folder breaks `IMPLEMENTATION_PLAN.md` section
`## 10. Immediate Next Steps (Week 1–2)` into smaller,
dispatchable work items.

## Execution order

### Week 1
1. `week-01/01-docs-metadata-and-changelog.md`
2. `week-01/02-core-parameters-2026-payroll.md`
3. `week-01/03-core-payroll-variables.md`
4. `week-01/04-core-payroll-tests-and-payslip-check.md`

### Week 2
5. `week-02/05-allowances-secondary-employer-and-regres.md`
6. `week-02/06-examples-and-template-cleanup.md`

## Dispatch rules

- Follow the task order unless a task explicitly says work can
  proceed in parallel.
- Read `IMPLEMENTATION_PLAN.md` before starting any task.
- Preserve OpenFisca conventions already documented in the plan:
  - Slovenian domain variable names
  - English explanatory comments wherever Slovenian is used in code
  - time-aware parameters with dated `values` entries
  - changelog maintenance with OpenFisca SemVer discipline
- Prefer small, reviewable commits/PRs: one task file should
  normally map to one PR.
- Do not delete placeholder content until the replacement
  tests/examples exist or the cleanup task explicitly authorises
  removal.

## Minimum handoff for every task

Each implementing agent should report:

- files created/edited
- open questions or legal/data uncertainties
- tests run
- whether `CHANGELOG.md` needs an entry now or in the integrating PR

## Repository anchors

- Main package: `openfisca_slovenia/`
- Parameters: `openfisca_slovenia/parameters/`
- Variables: `openfisca_slovenia/variables/`
- Tests: `openfisca_slovenia/tests/`
- Reforms: `openfisca_slovenia/reforms/`
- Examples: `openfisca_slovenia/situation_examples/`
- Project metadata: `README.md`, `pyproject.toml`, `CHANGELOG.md`
