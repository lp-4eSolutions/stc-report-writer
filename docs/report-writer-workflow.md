# Report writer workflow

This workflow is based only on files currently present in the repository, especially `office-scripts/RunReports.osts`, `office-scripts/AnalyseStyle.osts`, and the workbook template structure.

## End-to-end workflow

### 1. Prepare the template workbook

- Open `workbook/Form Tutor Report Writer (BLANK).xlsx` in Excel with Office Scripts support.
- Confirm the workbook contains the expected worksheets: `Settings`, `Reports`, `Lists`, `TokenLog`, and `Config`.
- Confirm named ranges exist for API/model settings, protection, flags, style settings, and report data ranges.
- Confirm no real school data is present before using or committing a workbook copy.

### 2. Configure settings

The workbook stores settings in named ranges rather than separate files. Evidenced named ranges include:

- API/model: `OpenAI_Key`, `Model`, `Temperature`, `FreqPenalty`, `PresPenalty`
- Protection/state: `ProtectionPwd`, `BusyFlag`, `RunReportsDefaults`
- Character limits: `CharLimitFlag`, `CharLimit`
- Style emulation: `EnableEmulation`, `StyleExamples`, `StyleProfileText`, `StyleProfileJSON`
- Data ranges: `ReportsData`, `RegenFlags`, `TokenCol`, `ExtraCurricList`, `RegenAll`
- Metadata/contact: `Version`, `ContactEmail`

No separate config files are currently present under `config/`.

### 3. Enter report input rows

The main generation script reads headers from the first row of the `Reports` worksheet and expects these columns:

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

Rows are considered data rows when the `ID` cell is non-blank. Completed rows are skipped unless `Regenerate` is true.

### 4. Optional style emulation

If style emulation is enabled:

1. The user enters one to five example reports in the `StyleExamples` named range.
2. The user runs `AnalyseStyle.osts`.
3. The script checks `EnableEmulation`.
4. It validates there is at least one example and no more than five.
5. It performs basic client-side regex replacement of obvious names with `[Name]`.
6. It sends the examples to the OpenAI API with instructions to focus on writing style rather than content.
7. It writes a style summary and JSON profile to the `Settings` worksheet via named ranges/cells.

If `RunReports.osts` later finds style emulation enabled but cannot parse a valid style profile, it writes an `ALERT` and leaves the row flagged for regeneration.

### 5. Generate reports

`RunReports.osts` performs the main generation flow:

1. Unprotects the workbook using `ProtectionPwd`.
2. Checks `BusyFlag` and exits if another run is already active.
3. Finds the `Reports` worksheet, `TokenLog` worksheet, and `RunReports` shape.
4. Captures/restores the button state and displays running/generating messages.
5. Builds a header-driven column map from the `Reports` first row.
6. Finds the last data row using non-blank `ID` values.
7. Counts rows requiring generation: rows with IDs whose status is not `Complete`, or rows with `Regenerate` true.
8. Processes each eligible row.

For each row, the script:

- validates that `Gender`, `Attitude`, and `Behaviour` are present;
- highlights missing required fields and writes `Missing: ...` statuses;
- handles `Off Site? = YES` with a local non-AI report sentence;
- maps attitude/behaviour values to descriptor words;
- cleans optional extracurricular, target, and comment strings;
- sends target/comment text to an AI sentiment-classification prompt;
- alerts if comment or target sentiment contradicts attitude/behaviour polarity;
- builds a report-generation prompt that requires exactly four sentences and JSON output;
- optionally applies style-profile and character-limit constraints;
- retries up to three times if the generated comment omits required descriptors;
- writes the final report, `Complete` status, token count, regeneration flag, and token-log row.

### 6. Supporting user actions and scripts

- `ClearAll.osts`: clears values in `ReportsData`, resets row heights, resets all `RegenFlags` to false, and preserves formatting/validation.
- `ToggleRegenAll.osts`: toggles regeneration for completed rows with reports.
- `ResetReports.osts`: restores the `RunReports` button state and clears `BusyFlag`.
- `SettingsButton.osts`: unhides/activates `Settings`.
- `ReturnToReports.osts`: hides `Settings` and activates `Reports`.
- `ToggleLock.osts`: toggles `Config` sheet visibility and `Reports` sheet protection.
- `CancelRun.osts`: sets a `CancelFlag` named range if present; current cancellation handling in the main script is not evidenced.

## Script/query/prompt/config interactions

- Office Scripts are the active automation layer evidenced in the repository.
- Prompts are embedded directly in Office Scripts; no standalone prompt files were present during the audit.
- Config values are stored in workbook named ranges; no standalone config files were present during the audit.
- No Power Query source files were present during the audit, and no workbook query/connection files were visible in the workbook package inspection.

## Expected outputs

- Generated report text in the `Report` column.
- `Complete`, `Missing: ...`, `ALERT`, or `ERROR: ...` values in the `Status` column.
- Token counts in the `Tokens` column when available.
- Regeneration flags in the `Regenerate` column.
- Token/status log entries in `TokenLog`.
- Style summary/profile output on `Settings` when style analysis is run.

## Known manual steps

- Supplying a valid OpenAI API key in the workbook.
- Assigning or confirming Office Script buttons/shapes in Excel.
- Verifying workbook protection and named ranges manually in Excel.
- Checking the workbook for hidden or cached sensitive data before committing.
- Refreshing any future Power Query queries if they are added.

## Not evidenced in repo

- Automated tests or CI.
- Standalone prompt/config files.
- Power Query source files.
- A complete local setup script.
- Definitive script assignment metadata for every workbook button/shape.
