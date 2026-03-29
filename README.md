# OpenFisca Slovenia

OpenFisca Slovenia is a tax-benefit microsimulation model for Slovenia,
built on the [OpenFisca](https://openfisca.org) framework.

> **Status — early development.**
> The package currently contains placeholder template legislation that is
> being progressively replaced with real Slovenian law. The immediate focus
> is on a **monthly employment payroll model** seeded with 2026 legislative
> parameters. See [`IMPLEMENTATION_PLAN.md`](IMPLEMENTATION_PLAN.md) for the
> full roadmap.

## What this package models

The initial implementation covers the **monthly payroll calculation** for a
salaried employee who is a tax resident in Slovenia:

- Gross salary (`bruto_placa`)
- Employee social security contributions (PIZ, zdravstvo, brezposelnost,
  starsevstvo, DO)
- Employer social security contributions
- Fixed health contribution (`OZP`)
- Personal income tax withholding (`akontacija_dohodnine`) with general and
  dependent allowances
- Net salary (`neto_placa`)
- Total employer cost (`strosek_delodajalca`)

All legislative parameters are modelled by **effective date** so that
any simulation date can be used — not just 2026.

## Package structure

```
openfisca_slovenia/
  parameters/       # Dated legislative rates and thresholds (YAML)
  variables/        # Formulas for each tax/benefit concept (Python)
  tests/            # YAML unit and integration tests
  reforms/          # Optional reform scenarios
  situation_examples/ # Example JSON situations for the Web API
```

## Install instructions

This package requires Python 3.9 or later (3.11 recommended).
All platforms that support Python are supported (Linux, macOS, Windows).

### A. Minimal installation (pip)

```sh
python -m venv .venv
source .venv/bin/activate   # Windows: .venv\Scripts\activate
pip install openfisca-slovenia
```

### B. Development installation (git clone)

```sh
git clone https://github.com/jausions/openfisca-slovenia
cd openfisca-slovenia
python -m venv .venv
source .venv/bin/activate   # Windows: .venv\Scripts\activate
pip install --upgrade pip build twine
pip install --editable ".[dev]" --upgrade
```

Run the test suite to verify the installation:

```sh
make test
```

Learn more about OpenFisca YAML tests:
https://openfisca.org/doc/coding-the-legislation/writing_yaml_tests.html

### C. Isolated testing with Tox

```sh
pipx install tox   # install tox once
tox                # run all environments
tox -p             # run in parallel
```

## Contributing

Read [`CONTRIBUTING.md`](CONTRIBUTING.md) for pull-request and changelog
conventions, and [`IMPLEMENTATION_PLAN.md`](IMPLEMENTATION_PLAN.md) for
the current roadmap and coding rules.

Key rules for contributors:

- Domain variable names are in **Slovenian**; every Slovenian term in code
  must have an English comment on the same line or the line above.
- Parameters use dated `values:` entries — never hardcode rates in formulas.
- Update `CHANGELOG.md` as part of every merged change.

## Serve the OpenFisca Web API

```sh
openfisca serve --port 5000 --country-package openfisca_slovenia
```

Test the API:

```sh
curl "http://localhost:5000/spec"
```

Send an example situation:

```sh
curl -X POST -H "Content-Type: application/json" \
  -d @./openfisca_slovenia/situation_examples/single.json \
  http://localhost:5000/calculate
```

## Distribution

Changes merged to `main` are automatically published to
[PyPI](https://pypi.org/project/openfisca-slovenia/) via GitHub Actions once
a `PYPI_TOKEN` secret is configured in the repository settings.

Manual publishing follows the
[Python Packaging Authority guidelines](https://packaging.python.org/en/latest/tutorials/packaging-projects/).
