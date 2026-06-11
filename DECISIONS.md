# Decisions and assumptions

This file records decisions evidenced by repository contents and documentation decisions made during maintenance. Do not record guesses as decisions.

## Evidenced by repository contents

- The project is workbook-centred: Office Scripts operate against worksheets, shapes, and named ranges in `workbook/Form Tutor Report Writer (BLANK).xlsx`.
- `RunReports.osts` is the main report-generation script and calls `https://api.openai.com/v1/chat/completions`.
- Report rows are identified by headers on the `Reports` worksheet rather than fixed column letters in the main generation script.
- The expected `Reports` headers are `ID`, `Name`, `Gender`, `Attitude`, `Behaviour`, `Extracurricular`, `Comment (optional)`, `Target`, `Off Site?`, `Report`, `Status`, `Tokens`, and `Regenerate`.
- The main script expects a `TokenLog` worksheet for timestamped token/status logging.
- The workbook stores runtime/config values in named ranges including `OpenAI_Key`, `Model`, `Temperature`, `FreqPenalty`, `PresPenalty`, `ProtectionPwd`, `BusyFlag`, `RunReportsDefaults`, `CharLimitFlag`, `CharLimit`, `EnableEmulation`, `StyleExamples`, `StyleProfileText`, and `StyleProfileJSON`.
- Style emulation is optional and depends on `EnableEmulation` plus a valid JSON style profile in `StyleProfileJSON`.
- `AnalyseStyle.osts` accepts one to five example reports, performs basic client-side name scrubbing, calls the OpenAI API, and writes a style summary/profile back to `Settings`.
- Free-text comments and targets are sent to an AI sentiment-classification prompt before report generation; contradictory sentiment creates an `ALERT` instead of a final report.
- Generated reports are requested as JSON with a single `comment` key and exactly four sentences.
- The current workbook is treated as a template only.

## Documentation decisions made on 2026-06-11

- Empty or placeholder folders (`config/`, `prompts/`, `power-query/`, `test-data/`) are documented as placeholders unless future source files are added.
- Documentation marks Power Query behaviour as not evidenced because no Power Query source files were present during the audit.
- Documentation avoids quoting workbook sample-report content and any report/free-text data.
- `.gitignore` was added to reduce accidental commits of live workbook copies, local exports, real data folders, credentials, and temporary Excel files.

## Assumptions future work depends on

- The checked-in workbook is intended to be a clean template, not a live working workbook.
- Any future test data should be synthetic or anonymised.
- If prompts are later extracted to `prompts/`, they should preserve the existing safety constraints unless a behaviour-changing task explicitly requires otherwise.

## Needs confirmation

- Whether `power-query/` is intentionally empty or whether workbook queries/connections are expected to be exported later.
- Whether prompt text should remain embedded in Office Scripts or be extracted into `prompts/`.
- Whether `config/` should contain a documented non-secret config template separate from workbook named ranges.
- Whether `test-data/` should contain a synthetic workbook/data fixture suite.
- Whether the sample style-emulation examples and existing workbook log entries are acceptable in the template workbook or should be removed before broader distribution.
- Whether the workbook's buttons/shapes are assigned to the intended scripts in Excel; this cannot be fully confirmed from the extracted workbook structure alone.
