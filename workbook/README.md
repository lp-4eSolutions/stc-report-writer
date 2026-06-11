# Workbook folder

This folder contains Excel workbook template files for the report writer.

## Purpose

The workbook is the user-facing Excel template for entering report inputs, storing workbook-side settings/named ranges, running Office Scripts, and receiving generated report output.

The current checked-in workbook is:

- `Form Tutor Report Writer (BLANK).xlsx`

Treat this file as a template workbook only.

## Strict data rules

Only clean anonymised template workbooks may be stored here.

Do not commit:

- live working copies;
- real pupil data;
- real staff data;
- SEND data;
- safeguarding data;
- behaviour incident data;
- assessment data;
- report comments;
- targets;
- generated reports from real pupils;
- free-text notes copied from school systems;
- API keys, credentials, or local secrets.

Use synthetic or anonymised examples only.

## Template workbook recommendation

The workbook currently contains non-empty template/config/sample/log content. Before wider use or redistribution, manually inspect it in Excel and decide whether it should:

1. remain in the repository as a clean anonymised template;
2. be replaced with a cleaner anonymised template; or
3. be removed from the repository and generated/distributed another way.

If any real or sensitive data is found, remove or replace the workbook and purge the sensitive data from repository history according to the organisation's incident process.

## Pre-commit workbook checklist

Before committing any workbook in this folder, check all of the following:

- [ ] The workbook is a template, not a live working copy.
- [ ] No real pupil, staff, SEND, safeguarding, behaviour, assessment, report, target, or free-text data is present.
- [ ] No real API key or credential is present.
- [ ] Hidden and very-hidden sheets have been inspected.
- [ ] Named ranges have been inspected for hidden values or secrets.
- [ ] Comments and notes have been inspected.
- [ ] Document properties/metadata have been cleared or verified safe.
- [ ] Cached query data and connections have been inspected.
- [ ] Token logs and error logs contain no real identifiers or sensitive data.
- [ ] Formulas do not reference local paths or private files.
- [ ] Buttons/shapes contain no sensitive labels or embedded data.
- [ ] No accidental pasted data exists outside the visible working ranges.
- [ ] The workbook opens cleanly and expected Office Scripts can be assigned manually in Excel.

## Related documentation

- `../docs/excel-workbook-setup.md`
- `../docs/report-writer-workflow.md`
- `../docs/data-protection-and-safeguards.md`
- `../docs/testing-checklist.md`
