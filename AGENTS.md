# Repository instructions for Codex agents

This repository supports an Excel/Office Scripts report writer for form tutor reports. Treat it as a school-data-adjacent project even when the checked-in data appears synthetic.

## Start-of-task reading

Before making changes, read these files if they exist:

1. `AGENTS.md`
2. `README.md`
3. `PROJECT_STATE.md`
4. `DECISIONS.md`
5. `CHANGELOG.md`

Also inspect the relevant files under `office-scripts/`, `power-query/`, `prompts/`, `config/`, `test-data/`, `docs/`, and `workbook/` before editing related documentation or behaviour.

## Safety and data-protection rules

- Preserve existing behaviour unless the user explicitly asks for behaviour changes.
- Preserve all safeguarding and data-protection checks.
- Do not weaken pupil-name blocking, free-text filtering, prompt constraints, sentiment checks, alert paths, or user-facing warning messages.
- Do not add real pupil data, staff data, SEND data, safeguarding data, behaviour data, report comments, assessment data, report outputs, or other sensitive school data.
- Use only synthetic, anonymised, or clearly fictional examples.
- Never commit live working workbooks, exported school datasets, API keys, local credentials, or identifiable school records.
- If a workbook appears to contain real or sensitive data, do not quote or summarise that data; document the repository risk only.
- Prefer config-driven rules over hard-coded school-specific logic.

## Office Scripts guidance

- Prefer bulk reads and writes over cell-by-cell operations.
- Avoid read methods inside loops where practical.
- Keep scripts robust against missing sheets, renamed columns, empty tables, and unexpected data.
- Keep worksheet/table/header interactions as header-driven as practical.
- Do not wrap imports in `try`/`catch` blocks.

## Power Query guidance

- Keep transformations readable.
- Avoid brittle column assumptions where practical.
- Document required source columns and manual refresh assumptions.

## Documentation maintenance

- Update `PROJECT_STATE.md` after each task with what changed, what was inspected, and remaining uncertainties.
- Update `DECISIONS.md` when a new evidenced decision, assumption, or constraint is introduced.
- Update `CHANGELOG.md` for behaviour-changing edits.
- For documentation-only changes, add a short documentation entry to `CHANGELOG.md`.
- Keep documentation evidence-based. Mark anything not confirmed by repository files as `Unknown`, `Needs confirmation`, or `Not evidenced in repo`.
