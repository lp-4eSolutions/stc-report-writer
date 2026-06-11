# STC Report Writer

An Excel-based form tutor report writer that uses Office Scripts and the OpenAI Chat Completions API to generate short yearly form-time report comments from workbook inputs.

This repository is intended to be the source of truth for the workbook template, Office Scripts, project documentation, and safe-development rules. It must not contain live school data.

## Project purpose

The checked-in Office Scripts indicate that the system is designed to:

- collect pupil/report inputs in an Excel workbook;
- validate required fields such as gender, attitude, and behaviour;
- detect contradictions between free-text comments/targets and attitude/behaviour grades;
- call the OpenAI API to generate a four-sentence form tutor report in JSON form;
- write generated reports, statuses, token counts, and alerts back into the workbook;
- optionally analyse up to five example reports to build a style profile for report generation.

## High-level workflow

1. A user works in the Excel template in `workbook/`.
2. The user enters or pastes report input rows on the `Reports` worksheet.
3. The user configures API/model/settings values through named ranges on `Config` and `Settings`.
4. Office Scripts in `office-scripts/` operate on workbook worksheets, named ranges, and shapes.
5. `RunReports.osts` validates rows, performs AI sentiment analysis for optional free text, requests a generated report, and writes results to the workbook.
6. Supporting scripts reset, clear, cancel, show/hide settings, toggle regeneration, and analyse writing style.

More detail is in `docs/report-writer-workflow.md`.

## Repository structure

| Path | Purpose |
| --- | --- |
| `office-scripts/` | Office Scripts exported as `.osts` JSON files. These are the only evidenced automation source files currently present. |
| `workbook/` | Excel template workbook and workbook-specific documentation. Only clean anonymised template workbooks should be stored here. |
| `docs/` | Project documentation, setup notes, workflow notes, safeguards, and testing checklist. |
| `config/` | Placeholder folder for external config files. Currently only contains `keep.gitkeep`; workbook config is currently evidenced through named ranges. |
| `prompts/` | Placeholder folder for prompt files. Currently only contains `keep.gitkeep`; prompts are currently embedded in Office Scripts. |
| `power-query/` | Placeholder folder for Power Query source. Currently only contains `keep.gitkeep`; no `.pq` or `.m` query source files are evidenced. |
| `test-data/` | Placeholder folder for synthetic test data. Currently only contains `keep.gitkeep`; no separate test dataset is evidenced. |
| `README.md` | Project overview and safe-development guide. |
| `PROJECT_STATE.md` | Current system understanding, inspected files, uncertainties, and next tasks. |
| `DECISIONS.md` | Evidenced project decisions and documentation decisions. |
| `CHANGELOG.md` | Notable changes. |
| `AGENTS.md` | Instructions for future Codex agents. |

## Setup assumptions

The repository does not currently contain a runnable local build or automated test harness. Evidence from the workbook and Office Scripts suggests these setup assumptions:

- Users need Excel for the web/Office Scripts support.
- Users need an OpenAI API key stored in the workbook named range `OpenAI_Key`.
- Model and generation settings are stored in workbook named ranges such as `Model`, `Temperature`, `FreqPenalty`, and `PresPenalty`.
- Workbook protection depends on the `ProtectionPwd` named range.
- The `Reports` worksheet must contain headers expected by `RunReports.osts`.
- The workbook contains buttons/shapes that are expected to be assigned to scripts in Excel.

Anything beyond these points is not evidenced in the repository and needs confirmation.

## Safe development rules

- Do not commit real pupil, staff, SEND, safeguarding, behaviour, assessment, report, target, comment, or other sensitive school data.
- Do not commit live working workbooks, API keys, exported MIS/SIMS files, cached AI outputs from real data, or local credentials.
- Use synthetic or anonymised examples only.
- Preserve existing validation, alert, prompt, and data-protection constraints unless explicitly asked to change them.
- Prefer documenting unknowns over guessing.
- Keep workbook files as clean templates only.

## Key files

- Main report generation script: `office-scripts/RunReports.osts`
- Style analysis script: `office-scripts/AnalyseStyle.osts`
- Supporting scripts: `office-scripts/ClearAll.osts`, `office-scripts/CancelRun.osts`, `office-scripts/ResetReports.osts`, `office-scripts/ReturnToReports.osts`, `office-scripts/SettingsButton.osts`, `office-scripts/ToggleLock.osts`, `office-scripts/ToggleRegenAll.osts`
- Template workbook: `workbook/Form Tutor Report Writer (BLANK).xlsx`
- Workbook documentation: `workbook/README.md`
- Workflow documentation: `docs/report-writer-workflow.md`
- Safeguards documentation: `docs/data-protection-and-safeguards.md`
- Workbook setup documentation: `docs/excel-workbook-setup.md`
- Testing checklist: `docs/testing-checklist.md`

## Data warning

No real school data should be committed to this repository. Before committing any workbook or data file, check hidden sheets, cached query data, comments, named ranges, workbook metadata, formulas, shapes, local exports, and accidental pasted data.
