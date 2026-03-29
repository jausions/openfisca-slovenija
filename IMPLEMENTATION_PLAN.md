# OpenFisca Slovenia — Implementation Plan

> Document version: 0.5.0 (2026-03-25)
> Status: Draft

---

## 1. Overview and v1 Scope

**Target**: a time-aware monthly employment payroll model for Slovenia. OpenFisca
natively supports parameter values that change over time; this package must use
that capability so that any year can be simulated by supplying the appropriate
dated parameter values.

**First encoded year**: **2026**. The initial implementation encodes 2026
legislation and rates as the seed dataset. This is the baseline for testing and
validation, not a permanent ceiling. New years are added by appending dated
entries to parameter YAML files — no code changes required.

**Population in scope**: salaried employees who are tax residents in Slovenia,
employed by a single employer, full-time.

**Out of scope for v1**: self-employment, annual tax reconciliation, collective
agreements, cross-border/secondment rules, non-employment social benefits,
multi-employer payrolls, and municipal surtaxes.

The existing code in this repository is a **template placeholder** describing a
fictional country. All existing variables, parameters, reforms, tests, and
situation examples must be reviewed and replaced with real Slovenian legislation.

---

## 2. Naming Policy

Based on the official OpenFisca naming guidelines at
https://openfisca.org/doc/contribute/variables-naming.html and
https://openfisca.org/doc/contribute/language.html.

### 2.1 Package and repository (English, unchanged)

| Asset                    | Name                   |
|--------------------------|------------------------|
| Repository               | `openfisca-slovenia`   |
| PyPI distribution        | `openfisca-slovenia`   |
| Python package directory | `openfisca_slovenia`   |
| Python module files      | English, `snake_case`  |

### 2.2 Variable names (Slovenian — local language rule)

The OpenFisca documentation explicitly states that for a country-specific
package the appropriate language for modelling the domain is one of the **native
languages of the modelled country**. For Slovenia that means **Slovenian**.

- Issues and pull requests → Slovenian preferred
- Variable names → Slovenian
- Parameter paths → Slovenian or English are both acceptable; prefer Slovenian
  domain terms where they are unambiguous
- Labels and metadata → Slovenian
- Code comments → Slovenian preferred, **with English explanations for non-Slovenian developers**
- Python infrastructure (`class`, `def`, module names) → English

### 2.2.1 English comment rule for code maintenance

Every Slovenian term, variable name, or concept used in code **must** be
accompanied by an inline English comment on the same line or immediately above.
This ensures that developers without Slovenian language knowledge can understand
the intent and maintain the code.

**Format:**

```python
# Determine bruto_placa (gross salary in EUR)
bruto_placa = base_salary + seniority_bonus

# Calculate prispevki_delavca (employee social contributions)
employee_contributions = gross_salary * 0.2310

# Apply splosna_olajava (general personal tax allowance)
taxable_base = max(0, income - allowance)
```

**Scope:**
- All variable names and parameter keys (at definition and first use in a section)
- All entity or attribute names
- Domain-specific acronyms (PIZ, DO, OZP, etc.) on first mention in a code block
- Mathematical or logical operations with semantic significance
- All docstrings for functions, classes, or modules using Slovenian terms

**Not required for:**
- Standard Python keywords and library functions
- Generic comments that do not reference Slovenian terms
- Comments already clear from context in a short file

This policy ensures that code reviews, debugging, and future refactoring remain
accessible to all team members, regardless of language background.

### 2.3 Acronyms

Accepted if broadly recognised and their meaning is findable by a Slovenian web
search. The following Slovenian legal acronyms are acceptable:

| Acronym | Full term                                             |
|---------|-------------------------------------------------------|
| `OZP`   | Obvezni zdravstveni prispevek                         |
| `PIZ`   | Pokojninsko in invalidsko zavarovanje                 |
| `DO`    | Dolgotrajna oskrba                                    |
| `ZDR`   | Zakon o delovnih razmerjih                            |

Avoid obscure internal three-letter abbreviations.

### 2.4 Scope prefix rule

When a prefix is needed to avoid ambiguity, put the **domain first**:

```
# Good
dohodnina_osnova
prispevki_delavca_zdravstvo

# Bad
osnova_dohodnina
zdravstvo_prispevki_delavca
```

### 2.5 Entity suffix rule

When the same concept exists at multiple entity levels, suffix with the entity:

```
bruto_placa_individual
bruto_placa_household
```

Do not mix prefixed and un-prefixed versions of the same variable family.

### 2.6 Reference variable name table

| Concept                          | Variable name                         |
|----------------------------------|---------------------------------------|
| Gross salary                     | `bruto_placa`                         |
| Base salary                      | `osnovna_placa`                       |
| Seniority allowance              | `dodatek_za_delovno_dobo`             |
| Performance pay                  | `delovna_uspesnost`                   |
| Overtime pay                     | `nadure`                              |
| Wage compensation                | `nadomestilo_place`                   |
| Meal allowance paid              | `malica_izplacano`                    |
| Meal allowance tax-free          | `malica_neobdavceno`                  |
| Meal allowance taxable           | `malica_obdavceno`                    |
| Transport reimbursement paid     | `povracilo_prevoza_izplacano`         |
| Transport reimbursement tax-free | `povracilo_prevoza_neobdavceno`       |
| Transport reimbursement taxable  | `povracilo_prevoza_obdavceno`         |
| Summer allowance paid            | `letni_regres_izplacano`              |
| Summer allowance tax-free        | `letni_regres_neobdavceno`            |
| Summer allowance taxable         | `letni_regres_obdavceno`              |
| Winter allowance paid            | `zimski_regres_izplacano`             |
| Winter allowance tax-free        | `zimski_regres_neobdavceno`           |
| Winter allowance taxable         | `zimski_regres_obdavceno`             |
| Employee SSC base                | `osnova_za_prispevke_delavca`         |
| Employee pension contribution    | `prispevki_delavca_PIZ`               |
| Employee health contribution     | `prispevki_delavca_zdravstvo`         |
| Employee unemployment contrib.   | `prispevki_delavca_brezposelnost`     |
| Employee parental contrib.       | `prispevki_delavca_starsevstvo`       |
| Employee long-term care contrib. | `prispevki_delavca_DO`                |
| Employee SSC total               | `prispevki_delavca`                   |
| Fixed health contribution        | `OZP`                                 |
| Employer SSC base                | `osnova_za_prispevke_delodajalca`     |
| Employer pension contribution    | `prispevki_delodajalca_PIZ`           |
| Employer health contribution     | `prispevki_delodajalca_zdravstvo`     |
| Employer unemployment contrib.   | `prispevki_delodajalca_brezposelnost` |
| Employer parental contrib.       | `prispevki_delodajalca_starsevstvo`   |
| Employer accident contrib.       | `prispevki_delodajalca_nezgode`       |
| Employer long-term care contrib. | `prispevki_delodajalca_DO`            |
| Employer SSC total               | `prispevki_delodajalca`               |
| Income tax base                  | `dohodnina_osnova`                    |
| General tax allowance            | `splosna_olajava`                     |
| Additional general allowance     | `dodatna_splosna_olajava`             |
| Dependent spouse allowance       | `olajava_za_zakonca`                  |
| First child allowance            | `olajava_za_prvega_otroka`            |
| Total tax allowances             | `skupne_olajave`                      |
| Income tax withholding           | `akontacija_dohodnine`                |
| Net salary                       | `neto_placa`                          |
| Cash to employee                 | `izplacilo_delavcu`                   |
| Employer total cost              | `strosek_delodajalca`                 |
| Is main employer                 | `je_glavni_delodajalec`               |
| Workdays in month                | `delovni_dnevi_v_mesecu`              |
| Months employed in year          | `meseci_zaposlitve`                   |

---

## 3. Entities

For v1, keep the entity model minimal:

| Entity      | Purpose                                             |
|-------------|-----------------------------------------------------|
| `Person`    | All individual-level payroll variables              |
| `Household` | Household-level context: spouse status, child count |

A `Person`-level flag `je_glavni_delodajalec` handles the main-employer vs
secondary-employer distinction without requiring a separate `Employer` entity.

Add an `Employment` entity in a later version only if multi-employer payroll or
concurrent-contract modelling becomes a real requirement.

---

## 4. Parameters

Organise parameters by payroll domain. Each file covers one concept and uses
**dated entries** so that annual updates are tracked without code changes.

### 4.0 Time-aware parameter convention

All parameter files must use OpenFisca's `values` block with ISO date keys so
that the engine resolves the correct value for any simulation date automatically:

```yaml
# Example: parameters/prispevki/delavec/PIZ.yaml
description: Stopnja prispevka za pokojninsko in invalidsko zavarovanje (PIZ) delavca
  # Employee pension and disability insurance (PIZ) contribution rate
values:
  2026-01-01:
    value: 0.1550
  # Add new entries here when rates change — no code changes required
```

When the first known effective date is earlier than 2026, encode it from that
date. When a rate is not yet known for a future year, leave it absent; the
engine will use the last known value until a new entry is added.

For parameters such as `minimalna_placa`, model values by **effective date**,
not as a year-locked constant. In practice this is often annual (January), but
dated entries must allow non-annual updates when legislation introduces them.

### 4.1 Suggested parameter tree

```
parameters/
  prispevki/
    delavec/
      PIZ.yaml
      zdravstvo.yaml
      brezposelnost.yaml
      starsevstvo.yaml
      DO.yaml
    delodajalec/
      PIZ.yaml
      zdravstvo.yaml
      brezposelnost.yaml
      starsevstvo.yaml
      nezgode.yaml
      DO.yaml
    OZP.yaml                     # fixed monthly amount + effective date ranges
  dohodnina/
    lestvica.yaml                # PIT brackets: thresholds, rates, constants
    splosna_olajava.yaml         # general allowance base + extra allowance formula
    olajave_za_vzdrzzevane.yaml  # dependent allowances (child, spouse)
  zaposlitev/
    minimalna_placa.yaml         # national minimum wage by effective date
    povprecna_placa.yaml         # national average gross wage placeholder (SURS)
    malica/
      dnevni_znesek.yaml         # tax-exempt cap per workday
      minimalne_ure.yaml         # eligibility threshold (4 hours)
    regres/
      letni_minimum.yaml         # summer allowance minimum (= min wage)
      zimski_minimum.yaml        # winter allowance minimum (= 0.5 × min wage)
      izjema_obsega.yaml         # exemption cap rules (= avg gross wage)
    bolnisca/
      stopnja_nadomestila.yaml   # employer sick pay rate (80%)
      stevilo_dni.yaml           # employer-covered period (30 days)
```

### 4.2 Seed parameter values (2026 — first encoded year)

The values below are the initial dataset to encode. They serve as the seed for
testing and validation. Future years are added by appending new dated entries to
the relevant YAML files.

#### Employee social contributions (on gross salary)

| Component           | Rate        |
|---------------------|-------------|
| PIZ (pension)       | 15.50 %     |
| Health              | 6.36 %      |
| Unemployment        | 0.14 %      |
| Parental/family     | 0.10 %      |
| Long-term care (DO) | 1.00 %      |
| **Total**           | **23.10 %** |

#### Employer social contributions (on gross salary)

| Component                | Rate        |
|--------------------------|-------------|
| PIZ (pension)            | 8.85 %      |
| Health                   | 6.56 %      |
| Accidents / occupational | 0.53 %      |
| Unemployment             | 0.06 %      |
| Parental/family          | 0.10 %      |
| Long-term care (DO)      | 1.00 %      |
| **Total**                | **17.10 %** |

#### Fixed compulsory health contribution (OZP)

| Period                         | Amount per month |
|--------------------------------|------------------|
| 2026-03-01 to 2027-02-28       | EUR 39.36        |

OZP is a **fixed flat amount**, never prorated. It is deducted by the main
employer only.

#### Personal income tax — annual brackets (2026)

Source: *Pravilnik za leto 2026*, Uradni list RS 104/2025.

| Annual taxable base (over – up to) | Rate | Cumulative constant |
|------------------------------------|------|---------------------|
| 0 – 9,721.43                       | 16 % | —                   |
| 9,721.43 – 28,592.44               | 26 % | 1,555.43            |
| 28,592.44 – 57,184.88              | 33 % | 6,461.89            |
| 57,184.88 – 82,346.23              | 39 % | 15,897.40           |
| Over 82,346.23                     | 50 % | 25,710.33           |

Monthly withholding uses each threshold and constant divided by 12 (1/12 rule).

#### Personal income tax — allowances (2026)

| Allowance                                                  | Annual                                | Monthly  |
|------------------------------------------------------------|---------------------------------------|----------|
| General allowance base (`GA_base`)                         | 5,551.93                              | 462.66   |
| Additional general allowance threshold                     | 17,766.18                             | 1,480.52 |
| Additional general allowance formula                       | `20,832.39 − 1.17259 × annual_income` |          |
| First dependent child                                      | 2,995.83                              | 249.65   |
| Other dependent family member (e.g. spouse with no income) | 2,995.83                              | 249.65   |

The additional general allowance applies only when estimated annual income is
at or below the threshold.

#### Employment parameters (2026)

| Parameter                              | Value                                            |
|----------------------------------------|--------------------------------------------------|
| National minimum wage (bruto, monthly) | 1,481.88 EUR                                     |
| National average gross wage (SURS ref) | ~2,791.04 EUR (Jan 2026; update at payment time) |
| Summer allowance minimum               | 1,481.88 EUR (= minimum wage)                    |
| Winter allowance minimum               | 740.94 EUR (= ½ minimum wage)                    |
| Meal allowance exempt cap              | 7.96 EUR/day                                     |
| Meal allowance eligibility             | ≥ 4 hours worked per day                         |
| Employer sick pay rate                 | 80 % of salary                                   |
| Employer sick pay period               | First 30 days                                    |

For each allowance/reimbursement that may be partly exempt, model dedicated
parameters for exemption caps and taxable excess treatment by effective date.
When PIT and SSC treatment differs, encode distinct PIT and SSC treatment
parameters instead of assuming they are identical.

---

## 5. Variables

### 5.1 Module structure

```
variables/
  zaposlitev.py      # employment inputs and earnings
  prispevki.py       # social contributions (employee + employer)
  dohodnina.py       # income tax withholding
  dodatki.py         # allowances and reimbursements (malica, regres, prevoz)
  bolnisca.py        # sick leave
  izplacilo.py       # net pay and employer cost outputs
```

**Important:** When implementing variables, every Slovenian term in the code must
have an accompanying English comment (see section 2.2.1 for format and scope).
Example docstring format:

```python
class bruto_placa(Variable):
    """Bruto plača (gross salary).
    
    Agreed monthly gross salary in EUR, before any deductions.
    Used as the contribution base for social security and tax calculations.
    """
    definition_period = MONTH
    entity = Person
    value_type = float
    default_value = 0
```

### 5.2 Inputs (Person-level)

```python
bruto_placa                    # agreed gross monthly salary
osnovna_placa                  # base salary component
dodatek_za_delovno_dobo        # seniority supplement (optional)
delovna_uspesnost              # performance-related pay (optional)
nadure                         # overtime pay (optional)
nadomestilo_place              # wage compensation (sick leave, holiday, etc.)
delovni_dnevi_v_mesecu        # working days in the month
malica_na_dan                  # meal allowance rate per workday (≤ cap)
povracilo_prevoza              # transport reimbursement input (monthly, employer-set);
                               # split outputs (izplacano/neobdavceno/obdavceno) are computed
je_glavni_delodajalec          # boolean: True if this is the main employer
meseci_zaposlitve              # months employed in the calendar year (for proration)
```

Household-level inputs:
```python
stevilo_otrok                  # number of dependent children
zakonec_brez_dohodka           # boolean: spouse with no income
```

### 5.3 Intermediate and output variables

#### Contributions

```python
osnova_za_prispevke_delavca    # = bruto_placa (contribution base)
prispevki_delavca_PIZ
prispevki_delavca_zdravstvo
prispevki_delavca_brezposelnost
prispevki_delavca_starsevstvo
prispevki_delavca_DO
prispevki_delavca              # sum of all employee contributions

OZP                            # fixed monthly flat contribution (main employer only)

osnova_za_prispevke_delodajalca  # = bruto_placa
prispevki_delodajalca_PIZ
prispevki_delodajalca_zdravstvo
prispevki_delodajalca_brezposelnost
prispevki_delodajalca_starsevstvo
prispevki_delodajalca_nezgode
prispevki_delodajalca_DO
prispevki_delodajalca          # sum of all employer contributions
```

#### Income tax withholding

```python
splosna_olajava                # general allowance (base + additional if eligible)
dodatna_splosna_olajava        # additional general allowance component
olajava_za_zakonca             # spouse dependent allowance (if eligible)
olajava_za_prvega_otroka       # first child allowance
skupne_olajave                 # sum of applicable allowances

dohodnina_osnova               # max(0, bruto_placa + malica_obdavceno
                               #   + povracilo_prevoza_obdavceno
                               #   + letni_regres_obdavceno
                               #   + zimski_regres_obdavceno
                               #   − prispevki_delavca − skupne_olajave)
akontacija_dohodnine           # monthly PIT withholding (main employer: bracket scale ÷ 12;
                               #   secondary employer: 25% flat of dohodnina_osnova)
```

#### Allowances and reimbursements

```python
malica_izplacano               # paid meal allowance
malica_neobdavceno             # tax-free meal allowance portion
malica_obdavceno               # taxable meal allowance portion

povracilo_prevoza_izplacano    # paid transport reimbursement
povracilo_prevoza_neobdavceno  # tax-free transport portion
povracilo_prevoza_obdavceno    # taxable transport portion

letni_regres_izplacano         # paid summer allowance (mandatory, prorated)
letni_regres_neobdavceno       # exempt summer allowance portion
letni_regres_obdavceno         # taxable summer allowance portion

zimski_regres_izplacano        # paid winter allowance (mandatory, prorated)
zimski_regres_neobdavceno      # exempt winter allowance portion
zimski_regres_obdavceno        # taxable winter allowance portion
```

For each receipt above, enforce identity:

`<prejemek>_izplacano = <prejemek>_neobdavceno + <prejemek>_obdavceno`.

Taxable portions (`*_obdavceno`) feed PIT and SSC bases according to dated
parameters for each concept.

#### Sick leave

```python
dni_bolniske                   # sick leave days in the period
nadomestilo_place_bolnisca     # employer-paid sick pay = 80% × daily_rate × dni_bolniske (first 30 days)
```

#### Outputs

```python
neto_placa                     # bruto_placa − prispevki_delavca − OZP − akontacija_dohodnine
izplacilo_delavcu              # neto_placa + malica_izplacano + povracilo_prevoza_izplacano
                               #   + letni_regres_izplacano + zimski_regres_izplacano
strosek_delodajalca            # bruto_placa + prispevki_delodajalca + malica_izplacano
                               #   + povracilo_prevoza_izplacano
                               #   + letni_regres_izplacano + zimski_regres_izplacano
```

---

## 6. Payslip Category Alignment

Per ZDR-1 Article 135, a Slovenian payslip must show the following categories
as visible (*razvidni*) data. The OpenFisca model should produce outputs that
map to these categories:

| ZDR-1 category                                 | Variables                                                                 |
|------------------------------------------------|---------------------------------------------------------------------------|
| Salary (`plača`)                               | `osnovna_placa`, `dodatek_za_delovno_dobo`, `nadure`, `delovna_uspesnost` |
| Wage compensation (`nadomestilo plače`)        | `nadomestilo_place`, `nadomestilo_place_bolnisca`                         |
| Reimbursements (`povračila stroškov`)          | `malica_izplacano`, `povracilo_prevoza_izplacano`                         |
| Other employment receipts (`drugi prejemki`)   | `letni_regres_izplacano`, `zimski_regres_izplacano`                       |
| Taxes and contributions (`davki in prispevki`) | All `prispevki_*`, `OZP`, `akontacija_dohodnine`                          |

---

## 7. Phased Implementation Milestones

### Milestone 1 — Core monthly payroll (v1)

Goal: simulate a realistic Slovenian monthly payslip for a full-time salaried
employee, without allowances or sick leave, using the seed parameter values.

Scope:
- taxable earnings (`bruto_placa` and components)
- employee social contributions (all components including DO)
- employer social contributions (all components including DO)
- fixed OZP contribution
- general and dependent allowances
- PIT withholding (main employer and secondary employer variants)
- net salary
- employer cost

Success criterion: given a gross salary, spouse status, and first-child flag,
the model produces the correct net salary and employer cost, matching a
manually verified payslip at the simulation date.

### Milestone 2 — Allowances and reimbursements (v1.1)

Goal: add mandatory payroll items beyond regular salary.

Scope:
- meal allowance (`malica_*`) with workday-based calculation, cap, and
  paid/exempt/taxable split (`malica_izplacano`, `malica_neobdavceno`,
  `malica_obdavceno`)
- transport reimbursement (`povracilo_prevoza_*`) with explicit
  paid/exempt/taxable split
- summer holiday allowance (`letni_regres_*`) with:
  - mandatory minimum (= national minimum wage)
  - proration for partial-year employment
  - paid/exempt/taxable split (`letni_regres_izplacano`,
    `letni_regres_neobdavceno`, `letni_regres_obdavceno`)
- winter allowance (`zimski_regres_*`) with:
  - mandatory minimum (= half national minimum wage)
  - proration
  - paid/exempt/taxable split (`zimski_regres_izplacano`,
    `zimski_regres_neobdavceno`, `zimski_regres_obdavceno`)

Success criterion: the model can simulate a payslip for a month in which a
regres payment is made, with correct exemption behaviour.

### Milestone 3 — Sick leave and wage compensations (v1.2)

Goal: add absence-related payroll items.

Scope:
- employer-paid sick leave: 80% of daily salary for first 30 days
- wage compensation outputs aligned with ZDR-1 category

Success criterion: the model correctly reduces net pay and employer cost when
sick leave days are present.

### Milestone 4 — Reforms and policy comparison

Goal: demonstrate OpenFisca's reform capability on the Slovenian model.

Suggested reform scenarios:
- change in PIT bracket rates or thresholds
- change in SSC rates
- change in meal allowance cap
- change in regres minimum

---

## 8. Test Strategy

### Unit tests (one-case-per-rule YAML files)

Cover each rule in isolation:

- each SSC component rate at a known gross salary
- employer SSC total
- OZP is flat and independent of salary
- PIT base floors at zero
- general allowance standard case
- additional general allowance applies below threshold, does not apply above
- spouse allowance applies only when eligible
- first-child allowance correct amount
- PIT bracket edges: just below and just above each threshold
- secondary employer uses 25% flat withholding
- meal allowance capped correctly
- meal allowance at exactly 4 hours/day eligibility boundary
- regres proration for 6 months employed
- regres exemption below and above the average gross wage cap
- decomposition identity per receipt:
  `izplacano = neobdavceno + obdavceno`
- one-cent above-cap tests for taxable excess
- PIT/SSC base propagation of `*_obdavceno` portions

### Integration tests (end-to-end payslip scenarios)

- single employee, no dependents
- married employee, non-working spouse, one child
- secondary employer
- employee receiving `letni_regres_izplacano` in July
  (with split exempt/taxable outputs)
- employee receiving `zimski_regres_izplacano` in December
  (with split exempt/taxable outputs)
- employee with 10 sick days in a month

### Regression tests

Once values are validated against official FURS examples or manually computed
payslips, freeze them as regression cases.

---

## 9. What to Defer Beyond v1/v1.1/v1.2

The following are explicitly deferred to avoid scope creep:

| Item                                                | Reason                                        |
|-----------------------------------------------------|-----------------------------------------------|
| Annual PIT reconciliation                           | Monthly withholding is the immediate priority |
| Municipal surtax (*pribitek*)                       | Varies by municipality; requires extra data   |
| Full dependent-child schedule (2nd, 3rd child etc.) | Only first-child confirmed in 2026 materials  |
| Multi-employer sequencing                           | Complex entity design; low priority for v1    |
| Dynamic SURS average wage integration               | Requires external data feed                   |
| Collective agreement supplements                    | Highly sector-specific                        |
| Cross-border / treaty rules                         | Out of scope for domestic payroll model       |
| Non-employment social benefits                      | Separate modelling domain                     |
| Payslip document rendering                          | Presentation layer, not legislation modelling |
| Annual summary payslip (due Jan 31)                 | Administrative output, not a legislative rule |
| Self-employment income                              | Different contribution and tax rules          |

### 9.1 Release and changelog discipline

Maintain `CHANGELOG.md` for every merged change, following the OpenFisca
country-package recommendation in `CONTRIBUTING.md`.

- Use **SemVer**:
  - **Patch**: fix or improve an existing calculation
  - **Minor**: add a new variable or user-facing feature
  - **Major**: rename or remove a public variable/API
- Treat the changelog update as part of the **definition of done** for changes
  to variables, parameters, entities, reforms, tests, or package metadata.
- Each changelog entry must include:
  - version number and pull request link in the heading
  - change type (`Tax and benefit system evolution`, `Technical improvement`,
    `Crash fix`, or `Minor change`)
  - impacted periods with exact dates when legislation/calculations change
  - impacted areas as repository-relative model paths
  - explicit user-facing details describing what changed and why it matters
- If one pull request contains several distinct user-visible changes, split them
  into separate changelog paragraphs using `<!-- -->`.

---

## 10. Immediate Next Steps (Week 1–2)

To make implementation easier to dispatch to coding agents, the week 1–2 work is
split into dedicated task briefs under `.agents/`:

- `.agents/README.md` — overview, sequencing, and handoff rules
- `.agents/week-01/01-docs-metadata-and-changelog.md`
- `.agents/week-01/02-core-parameters-2026-payroll.md`
- `.agents/week-01/03-core-payroll-variables.md`
- `.agents/week-01/04-core-payroll-tests-and-payslip-check.md`
- `.agents/week-02/05-allowances-secondary-employer-and-regres.md`
- `.agents/week-02/06-examples-and-template-cleanup.md`

Each task file defines scope, dependencies, target files, deliverables, and
acceptance criteria. Agents should follow the sequencing unless an earlier task
explicitly marks part of the work as unblockable in parallel.

### Days 1–3
- [ ] Update `README.md` to describe the real Slovenia package (remove the
  fictional country template text)
- [ ] Update `pyproject.toml` metadata (description, author, URLs)
- [ ] Adopt changelog discipline: update `CHANGELOG.md` with each merged change
  using OpenFisca SemVer and entry structure
- [ ] Create parameter files for employee SSC, employer SSC, DO, and OZP
- [ ] Create parameter files for 2026 PIT brackets and allowances
- [ ] Create parameter files for minimum wage and employment allowances
- [ ] Implement `bruto_placa`, `prispevki_delavca`, `prispevki_delodajalca`,
  `OZP` variables
- [ ] Implement `dohodnina_osnova` and `akontacija_dohodnine`
- [ ] Implement `neto_placa` and `strosek_delodajalca`

### Days 4–5
- [ ] Write YAML tests for each SSC component
- [ ] Write YAML tests for PIT bracket edges
- [ ] Write YAML tests for OZP flat behaviour
- [ ] Write YAML tests for dependent allowances
- [ ] Verify against a manually computed payslip (using 2026 seed parameters)

### Week 2
- [ ] Add `malica_izplacano`, `malica_neobdavceno`, `malica_obdavceno`
      and `povracilo_prevoza_*` equivalents
- [ ] Add `je_glavni_delodajalec` and secondary-employer flat withholding
- [ ] Add `letni_regres_*` and `zimski_regres_*` split variables (v1.1 scope)
- [ ] Write regres proration, exemption, and decomposition identity tests
- [ ] Write taxable-excess tests for malica and regres
- [ ] Add at least 5 complete situation examples in `situation_examples/`,
      including one with taxable excess on an allowance or regres
- [ ] Review and remove all placeholder template variables and tests

---

## 11. Reference Sources

| Source                                                | Used for                                    |
|-------------------------------------------------------|---------------------------------------------|
| SI-Employment-Taxes.fr.md v1.1.0 (2026-03-23)         | Core SSC rates, OZP, allowances, sick leave |
| resources/payslip_items/response_1.md + response_2.md | 2026 PIT brackets, OZP, allowance formulas  |
| resources/allowances/vacation_allowances_1.md + _2.md | Letni and zimski regres rules, exemptions   |
| ZDR-1 (Zakon o delovnih razmerjih)                    | Employment law, payslip requirements        |
| ZDoh-2 (Zakon o dohodnini)                            | PIT brackets, allowances                    |
| ZPZR (Zakon o zimskem regresu)                        | Winter allowance law (2025+)                |
| ZDOsk-1 (Zakon o dolgotrajni oskrbi)                  | DO contributions from July 2025             |
| ZZVZZ (Art. 48)                                       | OZP fixed contribution                      |
| FURS / SPOT                                           | Official rate confirmations                 |
| SURS (stat.si)                                        | Average gross wage reference for exemptions |
| OpenFisca naming guidelines                           | Variable naming rules                       |
| Pravilnik za leto 2026 (Uradni list RS 104/2025)      | 2026 PIT thresholds and allowances          |
