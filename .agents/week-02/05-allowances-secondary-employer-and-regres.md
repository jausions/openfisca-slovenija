# Task 05 — Allowances, Secondary Employer, and Regres

## Schedule

Week 2

## Goal

Extend the core payroll model with mandatory or common
non-base-salary items and the remaining employer-case logic
needed for a more realistic monthly payslip.

## Scope

- `malica`
- `povracilo_prevoza`
- `je_glavni_delodajalec` if not already implemented in week 1
- secondary-employer withholding behaviour if still partial
- `letni_regres`
- `zimski_regres`
- tests for proration and exemption limits

## Dependencies

- Requires week-1 parameters and core payroll variables
- Should complete before task 06 final examples/cleanup

## Target files

Likely edited/created:

- `openfisca_slovenia/variables/dodatki.py`
- `openfisca_slovenia/variables/dohodnina.py`
- `openfisca_slovenia/variables/izplacilo.py`
- `openfisca_slovenia/variables/zaposlitev.py`
- `openfisca_slovenia/tests/`
- `openfisca_slovenia/tests/situations/`
- relevant parameter files under `openfisca_slovenia/parameters/zaposlitev/`

## Implementation details

### 1. Meal allowance (`malica`)

Implement using:

- workday count in month
- per-day rate input or employer-set amount
- tax-exempt cap from parameters
- minimum-hours eligibility threshold

Model all three values explicitly:

- `malica_izplacano` (paid amount)
- `malica_neobdavceno` (exempt amount)
- `malica_obdavceno` (taxable excess)

Enforce identity in formulas/tests:
`malica_izplacano = malica_neobdavceno + malica_obdavceno`.

### 2. Transport reimbursement (`povracilo_prevoza`)

Implement the simplest rule set supported by confirmed
sources. If the exemption method is not yet fully sourced, keep
the variable modular and document the limitation rather than
guessing.

Even with simplified rules, expose split outputs:

- `povracilo_prevoza_izplacano`
- `povracilo_prevoza_neobdavceno`
- `povracilo_prevoza_obdavceno`

### 3. Main vs secondary employer

Ensure `je_glavni_delodajalec` is wired consistently:

- main employer uses allowance-aware bracket withholding and
  deducts `OZP`
- secondary employer uses the flat withholding path and should
  not duplicate main-employer-only logic if the legislation
  says so

### 4. `letni_regres` and `zimski_regres`

Implement:

- statutory minimums based on minimum wage fractions as
  defined in the plan
- proration by `meseci_zaposlitve`
- exemption ceilings based on average gross wage fractions
- mandatory split outputs for each receipt:
  - `*_izplacano`
  - `*_neobdavceno`
  - `*_obdavceno`
- clear PIT and SSC treatment of `*_obdavceno` portions using
  dated parameters

### 5. Testing focus

Add tests for:

- `malica` cap behaviour
- 4-hour eligibility boundary
- regres proration for partial-year employment
- exemption below/above threshold
- secondary-employer withholding interaction if changed in
  this task
- decomposition identity for each split receipt
- one-cent-above-cap cases that produce taxable excess
- PIT/SSC base propagation of `*_obdavceno` values

## Deliverables

- common allowances/reimbursements implemented
- secondary-employer path complete
- regres calculations and tests in place

## Acceptance criteria

- a month containing allowances or regres can be simulated end-to-end
- proration is tested
- exemption boundaries are tested
- unresolved transport-rule assumptions are documented
  explicitly instead of hardcoded silently
- split outputs (`*_izplacano`, `*_neobdavceno`, `*_obdavceno`)
  exist for `malica`, `povracilo_prevoza`, `letni_regres`,
  and `zimski_regres`

## Suggested verification

- run week-1 tests plus the new allowance/regres tests
- compare at least one regres month against a manual
  calculation

