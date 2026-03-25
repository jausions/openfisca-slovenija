# Task 03 — Core Payroll Variables

## Schedule

Week 1, Days 1–3

## Goal

Implement the milestone-1 payroll variables needed to calculate
employee deductions, employer contributions, income tax
withholding, net salary, and employer cost for a standard monthly
payslip.

## Scope

Implement or replace placeholder formulas for:

- gross salary input and contribution bases
- employee SSC components and total
- employer SSC components and total
- `OZP`
- tax allowances needed for withholding
- `dohodnina_osnova`
- `akontacija_dohodnine`
- `neto_placa`
- `strosek_delodajalca`

## Dependencies

- Requires task 02 parameter paths to exist or be agreed
- Should complete before task 04

## Target files

Likely new or heavily edited files:

- `openfisca_slovenia/variables/zaposlitev.py`
- `openfisca_slovenia/variables/prispevki.py`
- `openfisca_slovenia/variables/dohodnina.py`
- `openfisca_slovenia/variables/izplacilo.py`
- `openfisca_slovenia/variables/__init__.py`
- possibly `openfisca_slovenia/entities.py`

Legacy placeholder files to avoid extending unless strictly needed:

- `openfisca_slovenia/variables/benefits.py`
- `openfisca_slovenia/variables/housing.py`
- `openfisca_slovenia/variables/income.py`
- `openfisca_slovenia/variables/taxes.py`
- `openfisca_slovenia/variables/stats.py`

## Implementation details

### 1. Variable design principles

- Use Slovenian domain names from `IMPLEMENTATION_PLAN.md`
- Add English comments/docstrings wherever Slovenian terms are used
- Keep formulas simple and composable: one legal concept per variable where practical
- Avoid mixing milestone-2 features into milestone-1 formulas
  unless needed for compatibility

### 2. Minimum variable list

Inputs/intermediates/outputs to implement now:

- `bruto_placa`
- `osnova_za_prispevke_delavca`
- `prispevki_delavca_PIZ`
- `prispevki_delavca_zdravstvo`
- `prispevki_delavca_brezposelnost`
- `prispevki_delavca_starsevstvo`
- `prispevki_delavca_DO`
- `prispevki_delavca`
- `OZP`
- `osnova_za_prispevke_delodajalca`
- `prispevki_delodajalca_PIZ`
- `prispevki_delodajalca_zdravstvo`
- `prispevki_delodajalca_brezposelnost`
- `prispevki_delodajalca_starsevstvo`
- `prispevki_delodajalca_nezgode`
- `prispevki_delodajalca_DO`
- `prispevki_delodajalca`
- `splosna_olajava`
- `dodatna_splosna_olajava`
- `olajava_za_zakonca`
- `olajava_za_prvega_otroka`
- `skupne_olajave`
- `dohodnina_osnova`
- `akontacija_dohodnine`
- `neto_placa`
- `strosek_delodajalca`
- `je_glavni_delodajalec` if withholding logic needs it immediately

### 3. Tax logic expectations

- main employer: PIT bracket scale using the monthly equivalent
  of annual thresholds/constants
- secondary employer: flat 25% withholding on `dohodnina_osnova`
- base should floor at zero
- dependent/spouse allowances must be separable for testing

### 4. Entity/input expectations

If not already present, add minimal inputs for:

- `bruto_placa` on `Person`
- spouse-without-income and first-child context on the appropriate entity model
- `je_glavni_delodajalec` if used in formulas

Do not over-engineer new entities in this task.

### 5. Formula boundaries

Keep `malica`, `povracilo_prevoza`, `letni_regres`,
`zimski_regres`, and sick leave out of milestone-1 outputs
unless a zero-default placeholder is strictly needed.

## Deliverables

- milestone-1 payroll formulas implemented
- parameter lookups wired to the new Slovenia payroll parameter tree
- English explanatory comments present where Slovenian terms appear in code

## Acceptance criteria

- a monthly simulation can compute core deductions and net pay
  without relying on template-country logic
- main-employer and secondary-employer withholding paths are
  both supported or clearly stubbed with the required input in
  place
- formulas use dated parameters rather than hardcoded rates
- public variable names align with the plan naming table

## Suggested verification

- run targeted tests for the new variables once task 04 exists
- do a spot simulation for a simple employee case using 2026 seed values
- check edited Python files for errors after each edit batch

