# Changelog

All notable changes to `openfisca-slovenia` are documented here.

Follow the format described in [`CONTRIBUTING.md`](CONTRIBUTING.md):

- Heading = version number + PR link (heading level matches SemVer increment)
- Change type on the second line
- Impacted periods and areas for `Tax and benefit system evolution` entries
- Explicit user-facing details for all entries except `Minor change`

Separate unrelated changes in the same PR with `<!-- -->`.

---

## 0.1.0 - [#1](https://github.com/jausions/openfisca-slovenia/pull/1)

Minor change.

- Details:
  - Replaced template-country placeholder content with Slovenia-specific
    project documentation and metadata.
  - `README.md` now describes the Slovenian payroll model and its current
    implementation status instead of the fictional template country.
  - `pyproject.toml` updated: description, authors, maintainers,
    classifiers, version bumped to `0.1.0`.
  - Added `IMPLEMENTATION_PLAN.md` describing the payroll-first roadmap,
    naming conventions, parameter design, and test strategy.
  - Added `AGENTS.md` with coding rules for AI coding agents.
  - Added agent task-pack under `.agents/` (week-01 and week-02 briefs).
  - `CONTRIBUTING.md` template header note preserved; content unchanged.

<!-- -->

## 0.0.1 - [#0](https://github.com/openfisca/country-template/pull/0)

Tax and benefit system evolution.

- Impacted periods: all.
- Impacted areas:
  - `benefits`
  - `demographics`
  - `housing`
  - `income`
  - `stats`
  - `taxes`
- Details:
  - Initial import from OpenFisca country-package template.
    All variables and parameters are placeholder content and will be
    replaced by real Slovenian legislation.
