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
- Workbook package structure was inspected safely by reading workbook XML: sheet names, named ranges, row-one headers, hidden/visible states, data-validation ranges, formula locations, protection flags, drawing shape names, document property fields, custom XML/package parts, query/connection/table/comment parts, and non-empty cell counts. Real/sensitive row content was not reproduced in documentation.

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
- `docs/workbook-hygiene-audit.md`
- `workbook/README.md`
- `.gitignore`

## Latest workbook hygiene audit

- Performed a package/XML hygiene audit of `workbook/Form Tutor Report Writer (BLANK).xlsx` and documented the findings in `docs/workbook-hygiene-audit.md`.
- Inspected visible worksheet names and states, defined names, non-empty areas, formulas, data validation, comments/notes, tables, query/connection/external-link parts, drawing/shape names, document property fields, custom XML parts, and text patterns that could indicate API keys, local paths, private endpoints, email addresses, or high-risk school-data terms.
- The workbook did not evidence hidden or very-hidden sheets, comments, tables, queries, connections, external links, pivot parts, VBA parts, local paths, private endpoint patterns, or API-key-like values.
- The `ReportsData` and `TokenCol` named ranges appeared empty, and the `OpenAI_Key` named range contained a placeholder-like value rather than an API-key-like value.
- The workbook is not yet a safe blank template because style-emulation examples, style-profile output, and token/error log rows remain non-empty, and document/custom metadata still requires manual Excel confirmation.
- A prior package-level cleaning attempt was removed from the pull-request diff because the review/PR path reported that binary workbook files are not supported; this branch therefore documents the required cleanup but does not change the checked-in `.xlsx`.
- Current recommendation: keep the workbook only after specific cleaning through an approved binary-workbook route, or replace it with a cleaner blank template if the residue cannot be confidently cleared without breaking workbook structure.

## Latest documentation refinement

- Refined `.gitignore` so generic CSV/TSV exports remain ignored, while clearly named synthetic/example/template CSV fixtures under `test-data/` and example/template config CSVs under `config/` can be committed.

## Uncertainties needing user confirmation

- No standalone Power Query source files were present; Power Query behaviour is unknown.
- No standalone prompt files were present; prompt text is currently embedded in Office Scripts.
- No config source files were present; config is currently evidenced through workbook named ranges only.
- No test-data files were present; expected fixtures and regression tests need confirmation.
- The template workbook contains sample style-emulation text, style-profile output, and token-log/error entries. The package audit did not detect obvious API-key-like values, local paths, private endpoints, hidden sheets, or populated report-row data, but these non-empty areas must be cleaned or deliberately approved before the workbook is treated as a blank template.
- The workbook contains shapes/buttons and custom XML script-ID metadata, but exact script assignments cannot be fully confirmed from repository evidence alone.
- `CancelRun.osts` sets `CancelFlag` when it exists, but the audited workbook named ranges did not show `CancelFlag`, and `RunReports.osts` did not evidence cancel checkpoints.

## Recommended next implementation tasks

1. Clean or replace the checked-in workbook template through an approved binary-workbook route: clear style-emulation examples, style-profile output, and old token/error log rows while preserving named ranges, formulas, validation, protection, worksheet structure, and shape/script compatibility; then manually verify in Excel. If the PR system continues to reject `.xlsx` diffs, use manual Excel cleanup, a release artifact, or a repository-supported binary-file process instead of forcing the binary into this PR.
2. Export or document any Power Query queries/connections if they are part of the intended workflow.
3. Extract prompts into `prompts/` or document that prompts intentionally remain embedded in Office Scripts.
4. Add a synthetic test-data fixture set in `test-data/`.
5. Create a manual Office Scripts test protocol using the scenarios in `docs/testing-checklist.md`.
6. Confirm and, if needed, implement `CancelFlag` support consistently across workbook named ranges and `RunReports.osts`.
7. After cleaning, manually verify workbook metadata, custom XML/script assignments, printer/page setup, formulas, validation, protection, and shapes/buttons in Excel before redistributing the template.
8. Consider a lint/export process that converts `.osts` files to plain TypeScript for easier review while preserving Office Script import/export compatibility.
