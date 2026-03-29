# Task 06 — Examples and Template Cleanup

## Schedule

Week 2

## Goal

Finish the week-1/week-2 transition by adding realistic Slovenian
example situations and removing or retiring template-country
placeholder content that no longer matches the package
direction.

## Scope

- add at least 5 complete situation examples under `situation_examples/`
- review legacy template variables, parameters, reforms, and tests
- remove, replace, or clearly deprecate placeholder content once
  Slovenia replacements exist
- ensure examples and tests point to Slovenia payroll concepts
  rather than template-country benefits/taxes

## Dependencies

- Requires week-1 payroll model and tests
- Should run after task 05 so examples can include allowances/regres
  if desired
- Cleanup should be conservative: do not remove placeholders that are
  still needed to keep the package loadable until replacements are in
  place

## Target files

### New or updated examples

- `openfisca_slovenia/situation_examples/`
  - replace or supplement `single.json`
  - replace or supplement `couple.json`
  - replace or supplement `housing.json`
  - add new payroll-focused examples if clearer than reusing the template names

### Likely cleanup targets

#### Variables
- `openfisca_slovenia/variables/benefits.py`
- `openfisca_slovenia/variables/housing.py`
- `openfisca_slovenia/variables/income.py`
- `openfisca_slovenia/variables/stats.py`
- `openfisca_slovenia/variables/taxes.py`

#### Parameters
- `openfisca_slovenia/parameters/benefits/basic_income.yaml`
- `openfisca_slovenia/parameters/benefits/housing_allowance.yaml`
- `openfisca_slovenia/parameters/benefits/index.yaml`
- `openfisca_slovenia/parameters/benefits/parenting_allowance/`
- `openfisca_slovenia/parameters/taxes/housing_tax.yaml`
- `openfisca_slovenia/parameters/taxes/income_tax_rate.yaml`
- `openfisca_slovenia/parameters/taxes/social_security_contribution.yaml`

#### Tests
- `openfisca_slovenia/tests/basic_income.yaml`
- `openfisca_slovenia/tests/disposable_income.yaml`
- `openfisca_slovenia/tests/housing_allowance.yaml`
- `openfisca_slovenia/tests/housing_tax.yaml`
- `openfisca_slovenia/tests/income_tax.yaml`
- `openfisca_slovenia/tests/social_security_contribution.yaml`
- `openfisca_slovenia/tests/situations/income_tax.yaml`
- `openfisca_slovenia/tests/situations/parenting_allowance.yaml`

#### Reforms
- `openfisca_slovenia/reforms/add_dynamic_variable.py`
- `openfisca_slovenia/reforms/add_new_tax.py`
- `openfisca_slovenia/reforms/flat_social_security_contribution.py`
- `openfisca_slovenia/reforms/modify_social_security_taxation.py`
- `openfisca_slovenia/reforms/removal_basic_income.py`

## Implementation details

### 1. Situation examples

Create examples that are useful both for developers and future API
users. Prefer explicit Slovenian payroll cases over generic template
stories.

Minimum recommended example set:

1. single employee, main employer, no dependents
2. married employee, spouse without income, one dependent child
3. secondary-employer case with flat withholding
4. employee receiving `letni_regres`
5. employee with `malica` and `povracilo_prevoza`
6. at least one case with taxable excess on an allowance or regres
   (above exemption cap)

Each example should include:

- clear simulation date
- only required entities and inputs
- realistic payroll inputs based on the variables introduced in
  week 1–2
- enough expected outputs to show how the package is meant to be
  used
- split receipt outputs where relevant (`*_izplacano`,
  `*_neobdavceno`, `*_obdavceno`)

### 2. Cleanup decision rule

For every placeholder artifact, choose exactly one action and document
it in the PR/change summary:

- **remove** if no longer relevant and no other file depends on it
- **replace** if the same file path can now host Slovenia logic
- **keep temporarily** if needed for package stability, but add a
  note/TODO or track it explicitly for later removal

Avoid half-cleaned states where tests/examples still mention the
fictional template country after the Slovenia payroll path is in
place.

### 3. Reforms policy

Do not keep template reforms that describe fictional policies. Either:

- remove them, or
- replace them with Slovenia-relevant demonstration reforms from
  milestone 4

If no Slovenia-specific reform is ready yet, it is acceptable to remove
placeholder reform files and leave the package without reforms for
now.

### 4. Consistency checks

After cleanup:

- `README.md` examples must not point to removed template concepts
- `CHANGELOG.md` should mention the cleanup if it is user-visible
- imports/registrations in `__init__.py` or module exports must not
  reference removed files
- tests should fail only for real implementation gaps, not because of
  dangling template references

## Deliverables

- realistic Slovenia payroll situation examples
- placeholder template artifacts removed or explicitly retired
- repository narrative aligned across examples, tests, parameters, variables, and reforms

## Acceptance criteria

- at least 5 complete Slovenia-specific situation examples exist
- at least one example includes taxable excess with split outputs
  (`*_izplacano`, `*_neobdavceno`, `*_obdavceno`)
- no prominent user-facing file still presents the fictional template
  legislation as current behaviour
- removed placeholder files are not still imported or referenced
- cleanup does not break package loading or the active test suite

## Suggested verification

- list remaining references to `basic_income`, `housing_allowance`, and other template concepts
- run relevant tests after cleanup
- manually inspect the example JSON files for readability and realism

