# Task 04 — Core Payroll Tests and Payslip Check

## Schedule

Week 1, Days 4–5

## Goal

Turn the milestone-1 payroll model into a validated baseline by
writing rule-level YAML tests, end-to-end payslip scenarios, and
one manual cross-check against a trusted payslip calculation.

## Scope

- unit-style YAML tests for each core payroll rule
- integration-style scenarios for main employer / secondary
  employer / dependent allowances
- one manually verified payslip check using the 2026 seed parameters

## Dependencies

- Requires task 02 parameters
- Requires task 03 variables

## Target files

- `openfisca_slovenia/tests/`
- `openfisca_slovenia/tests/situations/`
- optionally a short verification note under `tasks/agents/`
  or another agreed docs path if the manual payslip calculation
  needs to be recorded

Legacy placeholder tests that should not define the target model for this task:

- `openfisca_slovenia/tests/basic_income.yaml`
- `openfisca_slovenia/tests/housing_allowance.yaml`
- `openfisca_slovenia/tests/housing_tax.yaml`
- `openfisca_slovenia/tests/income_tax.yaml`
- `openfisca_slovenia/tests/social_security_contribution.yaml`

## Implementation details

### 1. Unit-rule coverage

Add focused YAML tests for:

- each employee SSC component rate
- each employer SSC component rate
- employee and employer totals
- `OZP` flat amount behaviour
- `dohodnina_osnova` flooring at zero
- `splosna_olajava` standard case
- additional general allowance below and above threshold
- spouse allowance eligibility
- first-child allowance amount
- PIT bracket edges around each threshold
- secondary-employer 25% flat withholding

For week-2 readiness, reserve test structure for split receipts
(`*_izplacano`, `*_neobdavceno`, `*_obdavceno`) so taxable excess
and decomposition identity are easy to add without test rewrites.

### 2. Integration scenarios

Add end-to-end cases for at least:

- single employee, no dependents
- spouse without income
- first child allowance
- secondary employer

Keep scenario inputs minimal and readable.

### 3. Manual verification

Perform one full manual calculation for a realistic monthly
payslip using the seed values.

Record:

- chosen gross salary and household assumptions
- expected SSC breakdown
- expected PIT base and withholding
- expected net salary
- expected employer cost
- any assumptions or unresolved legal interpretation points

### 4. Test style

- prefer one rule per file or one small cluster per file
- use explicit dates in test inputs
- avoid inheriting template-country expectations

## Deliverables

- milestone-1 YAML tests
- at least one manually checked baseline payslip
- enough coverage to catch regressions while implementing week 2

## Acceptance criteria

- every core payroll component has at least one direct test
- bracket-edge behaviour is covered
- `OZP` behaviour is proven to be salary-independent
- at least one integrated scenario validates the full payslip chain
- manual verification results are recorded in a place future
  agents can find
- test layout is compatible with split allowance variables to be
  introduced in week 2

## Suggested verification

- run the full YAML test suite relevant to the new payroll model
- spot-check failures to ensure they come from legislation
  logic, not bad test wiring

