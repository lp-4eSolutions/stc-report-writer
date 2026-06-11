# Project state

Last updated: 2026-06-11

## What was inspected

- Top-level documentation files: `AGENTS.md`, `README.md`, `PROJECT_STATE.md`, `DECISIONS.md`, and `CHANGELOG.md`.
- Office Script exports in `office-scripts/`:
  - `AnalyseStyle.osts`
  - `CancelRun.osts`
  - `ClearAll.osts`
  - `ResetReports.osts`
  - `ReturnToReports.osts`
  - `RunReports.osts`
  - `SettingsButton.osts`
  - `ToggleLock.osts`
  - `ToggleRegenAll.osts`
- Placeholder/source folders: `config/`, `docs/`, `power-query/`, `prompts/`, and `test-data/`.
- Workbook folder and template workbook: `workbook/Form Tutor Report Writer (BLANK).xlsx` and `workbook/README.md`.
- Workbook package structure was inspected safely by reading workbook XML: sheet names, named ranges, row-one headers, data-validation ranges, formula locations, protection flags, drawing shape names, and non-empty cell counts. Real/sensitive row content was not reproduced in documentation.

## Current understanding of the system

This is an Excel/Office Scripts report writer for form tutor reports. The workbook supplies pupil/report input rows and configuration values through worksheets and named ranges. Office Scripts read those values, call the OpenAI Chat Completions API, and write generated report comments, statuses, token counts, and alerts back to the workbook.

The main workflow is driven by `office-scripts/RunReports.osts`. Supporting scripts clear/reset the workbook state, cancel a run, show/hide settings, toggle regeneration flags, and analyse writing style examples.

## Key files and folders

- `workbook/Form Tutor Report Writer (BLANK).xlsx`: checked-in Excel template workbook.
- `office-scripts/RunReports.osts`: main report generation script.
- `office-scripts/AnalyseStyle.osts`: optional style-profile generation script.
- `office-scripts/ClearAll.osts`: clears input values and regeneration flags while preserving formatting/validation.
- `office-scripts/CancelRun.osts`: sets `CancelFlag` if the named range exists; no current `RunReports.osts` checkpoint for `CancelFlag` was evidenced during this audit.
- `office-scripts/ResetReports.osts`: restores the `RunReports` button and clears `BusyFlag`.
- `office-scripts/SettingsButton.osts` and `office-scripts/ReturnToReports.osts`: navigate between `Settings` and `Reports` while changing `Settings` visibility.
- `office-scripts/ToggleRegenAll.osts`: toggles regeneration flags on complete report rows.
- `office-scripts/ToggleLock.osts`: toggles `Config` sheet visibility and `Reports` protection.
- `config/`, `power-query/`, `prompts/`, `test-data/`: currently placeholder folders with only `.gitkeep` files.

## Current workflow

1. A user opens the template workbook.
2. A user enters report-row inputs on `Reports`.
3. The workbook provides API/model/settings through named ranges on `Config` and `Settings`.
4. Optional: a user enables style emulation and runs `AnalyseStyle.osts` after entering one to five example reports in `StyleExamples`.
5. A user runs the report-generation script from the `RunReports` button/shape.
6. `RunReports.osts` checks `BusyFlag`, reads workbook headers, identifies rows with IDs, skips completed rows unless `Regenerate` is true, and validates required fields.
7. It sends optional comment/target text to a sentiment prompt and alerts if sentiment contradicts attitude/behaviour grades.
8. It builds a report prompt requiring exactly four sentences in JSON output, optionally constrained by style-emulation and character-limit settings.
9. It writes generated report text, status, token count, regeneration state, and token log rows back to the workbook.
10. Reset/clear/navigation scripts maintain workbook state and UI.

## Current known safeguards

- Required-field checks for gender, attitude, and behaviour.
- Status and fill-colour warnings for missing required values.
- Off-site shortcut produces a non-AI attendance statement and zero token count.
- AI sentiment classification checks optional comments and targets against attitude/behaviour polarity.
- Contradictions produce `ALERT` statuses and keep regeneration enabled.
- Prompt constraints prohibit inventing/embellishing beyond provided information.
- Generated report output must be JSON with only a `comment` key.
- Generated reports are retried up to three times if required descriptors are missing.
- Style-analysis examples are limited to one to five reports and undergo basic regex name scrubbing before the API call.
- Workbook and sheet protection are used in several scripts via the `ProtectionPwd` named range.

## Documentation created or updated

- `AGENTS.md`
- `README.md`
- `PROJECT_STATE.md`
- `DECISIONS.md`
- `CHANGELOG.md`
- `docs/report-writer-workflow.md`
- `docs/data-protection-and-safeguards.md`
- `docs/excel-workbook-setup.md`
- `docs/testing-checklist.md`
- `workbook/README.md`
- `.gitignore`

## Latest documentation refinement

- Refined `.gitignore` so generic CSV/TSV exports remain ignored, while clearly named synthetic/example/template CSV fixtures under `test-data/` and example/template config CSVs under `config/` can be committed.

## Uncertainties needing user confirmation

- No standalone Power Query source files were present; Power Query behaviour is unknown.
- No standalone prompt files were present; prompt text is currently embedded in Office Scripts.
- No config source files were present; config is currently evidenced through workbook named ranges only.
- No test-data files were present; expected fixtures and regression tests need confirmation.
- The template workbook contains sample style-emulation text and token-log/error entries; these appear non-live from limited inspection, but their intended presence in the distributable template needs confirmation.
- The workbook contains shapes/buttons, but exact script assignments cannot be fully confirmed from repository evidence alone.
- `CancelRun.osts` sets `CancelFlag` when it exists, but the audited workbook named ranges did not show `CancelFlag`, and `RunReports.osts` did not evidence cancel checkpoints.

## Recommended next implementation tasks

1. Decide whether to keep, clean, or replace the checked-in workbook template after a manual Excel inspection of hidden metadata, cached values, and shape/script assignments.
2. Export or document any Power Query queries/connections if they are part of the intended workflow.
3. Extract prompts into `prompts/` or document that prompts intentionally remain embedded in Office Scripts.
4. Add a synthetic test-data fixture set in `test-data/`.
5. Create a manual Office Scripts test protocol using the scenarios in `docs/testing-checklist.md`.
6. Confirm and, if needed, implement `CancelFlag` support consistently across workbook named ranges and `RunReports.osts`.
7. Review style-emulation sample text and token-log entries in the workbook template and remove them if the template should be completely blank.
8. Consider a lint/export process that converts `.osts` files to plain TypeScript for easier review while preserving Office Script import/export compatibility.
