# Task 02 — Core Parameters (Seed Payroll Dataset)

## Schedule

Week 1, Days 1–3

## Goal

Create the first real Slovenia payroll parameter set, using dated
entries so the model stays time-aware and can later absorb new
legislative values without code changes.

## Scope

Create or reorganise parameter files for:

- employee SSC rates
- employer SSC rates
- long-term care (`DO`) rates
- fixed health contribution (`OZP`)
- PIT brackets and constants
- PIT allowances
- national minimum wage
- employment-related caps/minima needed by week-1 and week-2 variables

## Dependencies

- Read `IMPLEMENTATION_PLAN.md` sections 4 and 7
- Coordinate lightly with task 03 on final parameter paths/names if needed
- Should land before or together with task 03

## Target files

Primary destination should converge toward the structure defined in
`IMPLEMENTATION_PLAN.md`:

- `openfisca_slovenia/parameters/prispevki/...`
- `openfisca_slovenia/parameters/dohodnina/...`
- `openfisca_slovenia/parameters/zaposlitev/...`

Existing placeholder files likely to be replaced or retired later:

- `openfisca_slovenia/parameters/benefits/basic_income.yaml`
- `openfisca_slovenia/parameters/benefits/housing_allowance.yaml`
- `openfisca_slovenia/parameters/taxes/housing_tax.yaml`
- `openfisca_slovenia/parameters/taxes/income_tax_rate.yaml`
- `openfisca_slovenia/parameters/taxes/social_security_contribution.yaml`

## Implementation details

### 1. Parameter design rules

- Use OpenFisca `values:` blocks with ISO dates
- Use effective dates, not year labels encoded in file names
- Prefer one concept per file
- Add clear descriptions
- When Slovenian terms appear in descriptions/comments, add an
  English explanation nearby

### 2. Minimum parameter set for milestone 1

Create dated values for:

- `prispevki/delavec/PIZ.yaml`
- `prispevki/delavec/zdravstvo.yaml`
- `prispevki/delavec/brezposelnost.yaml`
- `prispevki/delavec/starsevstvo.yaml`
- `prispevki/delavec/DO.yaml`
- `prispevki/delodajalec/PIZ.yaml`
- `prispevki/delodajalec/zdravstvo.yaml`
- `prispevki/delodajalec/brezposelnost.yaml`
- `prispevki/delodajalec/starsevstvo.yaml`
- `prispevki/delodajalec/nezgode.yaml`
- `prispevki/delodajalec/DO.yaml`
- `prispevki/OZP.yaml`
- `dohodnina/lestvica.yaml`
- `dohodnina/splosna_olajava.yaml`
- `dohodnina/olajave_za_vzdrzzevane.yaml`
- `zaposlitev/minimalna_placa.yaml`

If week-2 work is easier with them already present, also add
placeholders/seed values for:

- `zaposlitev/povprecna_placa.yaml`
- `zaposlitev/malica/dnevni_znesek.yaml`
- `zaposlitev/malica/minimalne_ure.yaml`
- `zaposlitev/regres/letni_minimum.yaml`
- `zaposlitev/regres/zimski_minimum.yaml`
- `zaposlitev/regres/izjema_obsega.yaml`
- `zaposlitev/bolnisca/stopnja_nadomestila.yaml`
- `zaposlitev/bolnisca/stevilo_dni.yaml`

### 3. PIT structure

Model the PIT scale so task 03 can compute monthly withholding cleanly:

- annual thresholds
- annual rates
- annual cumulative constants
- explicit note in description or task handoff that monthly
  withholding uses the 1/12 rule

### 4. Parameter naming

Keep filenames stable and domain-oriented. Do not bake `2026` into filenames.

### 5. Migration discipline

Do not spend time deleting all placeholder parameter files in this
task unless removal is needed to unblock tests. Record obsolete
paths for task 06 cleanup.

## Deliverables

- seed payroll parameter tree created
- values dated and ready for time-aware lookup
- parameter names aligned with the implementation plan

## Acceptance criteria

- all milestone-1 parameter dependencies are available under stable paths
- parameter files use valid OpenFisca YAML structure
- `minimalna_placa` is modeled by effective date, not as a year-locked concept
- no new parameter file is named after a specific year
- enough descriptions/context exist for non-Slovenian maintainers
  to follow the model

## Suggested verification

- load or inspect the YAML files for syntax issues
- ensure paths referenced by formulas in task 03 exist exactly as named
- if possible, run a minimal simulation/read of a parameter value
  at a 2026 date

