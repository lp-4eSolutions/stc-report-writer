# Excel workbook setup

This document describes the workbook structure evidenced by inspecting `workbook/Form Tutor Report Writer (BLANK).xlsx` as a template workbook. Do not store live working copies in the repository.

## Workbook role

The workbook is the user-facing Excel template for entering form tutor report inputs, configuring API/settings values, running Office Scripts, and receiving generated report outputs.

The checked-in workbook must be treated as a clean anonymised template only.

## Expected worksheets

The inspected template workbook contains these worksheets:

| Worksheet | Evidence-based role |
| --- | --- |
| `Settings` | User settings for character limit and style emulation; stores style summary/profile output. |
| `Reports` | Main report input/output sheet. |
| `Lists` | Validation/support lists such as extracurricular activities and attitude/behaviour scores. |
| `TokenLog` | Log sheet used by `RunReports.osts` for timestamp, pupil/report ID, state, and token/error values. |
| `Config` | API/model/protection/version/contact/defaults values. |

## Expected `Reports` columns

`RunReports.osts` builds a header-driven map and expects the following headers on the `Reports` worksheet:

- `ID`
- `Name`
- `Gender`
- `Attitude`
- `Behaviour`
- `Extracurricular`
- `Comment (optional)`
- `Target`
- `Off Site?`
- `Report`
- `Status`
- `Tokens`
- `Regenerate`

The inspected workbook row-one headers also include blank spacer columns between some input/output areas. The script uses header names, so spacer columns should not matter as long as the required headers are present and unique.

## Expected named ranges

The inspected workbook defines these named ranges:

| Name | Refers to | Evidence-based purpose |
| --- | --- | --- |
| `OpenAI_Key` | `Config!$A$1` | API key placeholder/input. |
| `Model` | `Config!$A$2` | OpenAI model name. |
| `Temperature` | `Config!$A$3` | Generation temperature. |
| `FreqPenalty` | `Config!$A$4` | Frequency penalty. |
| `PresPenalty` | `Config!$A$5` | Presence penalty. |
| `Version` | `Config!$A$6` | Workbook/project version value. |
| `ProtectionPwd` | `Config!$A$17` | Password used by protection/unprotection scripts. |
| `RunReportsDefaults` | `Config!$A$24` | JSON snapshot of the `RunReports` button's default appearance/position. |
| `ContactEmail` | `Config!$H$1` | Contact hyperlink/email area. |
| `CharLimitFlag` | `Settings!$B$1` | Character-limit enable flag. |
| `CharLimit` | `Settings!$C$1` | Character-limit value. |
| `EnableEmulation` | `Settings!$B$3` | Style-emulation enable flag. |
| `StyleProfileText` | `Settings!$D$3` | Style summary output. |
| `StyleProfileJSON` | `Settings!$F$3` | Style profile JSON output. |
| `StyleExamples` | `Settings!$B$4:$F$4` | Up to five example report inputs. |
| `ExtraCurricList` | `Lists!$A$2:$A$15` | Extracurricular validation/support list. |
| `ReportsData` | `Reports!$B$4:$P$503` | Main clearable report data range. |
| `RegenFlags` | `Reports!$Q$4:$Q$503` | Regeneration flags reset by `ClearAll.osts`. |
| `TokenCol` | `Reports!$P$4:$P$503` | Token output column range. |
| `RegenAll` | `Reports!$Q$1` | Regeneration header/control cell. |
| `BusyFlag` | `Reports!$CD$102` | Run-in-progress flag. |

## Tables and Power Query

No Excel table definitions were found in the workbook XML inspection. No workbook query/connection parts were visible in the package inspection, and the repository's `power-query/` folder currently contains no query source files.

Power Query setup is therefore `Not evidenced in repo` and needs confirmation.

## Data validation and formulas

Inspection found workbook data-validation ranges on `Settings` and `Reports`, including validation for the character-limit cell and several report-input areas. These should be checked manually in Excel because validation formulas/details are easier to verify in the Excel UI than in documentation.

Inspection also found formulas in workbook cells, including formulas on `Reports` for generated/structured IDs and formulas on `Config`/`Settings`. Do not replace the workbook without preserving formulas that the scripts depend on.

## Buttons/shapes and script setup

The workbook package includes drawing shapes with names including:

- `RunReports`
- `SelectCompletedButton`
- `AnalyseStyleButton`
- several rounded rectangles/pictures used for UI

`RunReports.osts` specifically looks for a shape named `RunReports` and changes its text/format during a run. Exact Office Script assignments for buttons must be checked manually in Excel.

Expected manual assignments, based on script names and comments, are likely:

- `RunReports` shape -> `RunReports.osts`
- style-analysis button -> `AnalyseStyle.osts`
- settings/navigation buttons -> `SettingsButton.osts` and `ReturnToReports.osts`
- clear/reset/regenerate controls -> matching support scripts

These assignments are `Needs confirmation` in Excel.

## Protection setup

The inspected workbook includes sheet protection on `Settings` and `Reports`, and scripts use the `ProtectionPwd` named range to unprotect/protect the workbook or sheets. Keep protection settings aligned with the scripts.

## Refresh/run order

A conservative evidenced run order is:

1. Open a clean template workbook copy.
2. Enter the API key/model/settings in the workbook.
3. Enter synthetic or approved working report inputs on `Reports`.
4. Optional: enable style emulation, enter one to five example reports, and run `AnalyseStyle.osts`.
5. Run `RunReports.osts`.
6. Review `Status`, `Report`, `Tokens`, and `TokenLog`.
7. Use `ToggleRegenAll.osts`, individual `Regenerate` flags, or `ClearAll.osts` as needed.
8. Use `ResetReports.osts` if the run button/busy state needs restoration.

## Workbook template rules

- Store only clean anonymised template workbooks in `workbook/`.
- Do not commit live working copies.
- Do not commit real pupil, staff, SEND, safeguarding, behaviour, assessment, target, report, or free-text data.
- Do not commit real API keys or credentials.
- Before committing a workbook, inspect hidden sheets, named ranges, comments, workbook/document properties, cached query data, token logs, formulas, shapes, and accidental pasted values.

## Manual checks required in Excel

- Confirm all named ranges exist and point to expected cells/ranges.
- Confirm required headers are present on `Reports`.
- Confirm data validation dropdowns and formulas behave as intended.
- Confirm buttons/shapes are assigned to the correct Office Scripts.
- Confirm protection/unprotection works with `ProtectionPwd`.
- Confirm no real or sensitive data is present in visible sheets, hidden sheets, comments, document properties, cached data, or logs.
