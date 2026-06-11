# Data protection and safeguards

This document records the safeguards evidenced in the repository and identifies gaps that need confirmation or improvement. Do not treat this as legal advice or a complete DPIA.

## Data handled by the system

The workbook/script workflow is designed to process school-report-related data including pupil names, gender/pronouns, attitude and behaviour grades, extracurricular involvement, optional comments, targets, generated report text, statuses, and token logs.

Because this project is school-data-adjacent, all development and testing must use synthetic or anonymised data only.

## Current evidenced safeguards

### Required-field validation

`RunReports.osts` requires `Gender`, `Attitude`, and `Behaviour` before generating an AI report. If any are missing, it writes a `Missing: ...` status, highlights the status and missing input cells, clears report/token output, and does not call the report-generation prompt for that row.

### Off-site shortcut

If `Off Site?` is `YES`, the script writes a local attendance sentence, marks the row complete, sets token count to zero, clears regeneration, and does not call the AI report-generation prompt for that row.

### Free-text sentiment filtering

Optional `Comment (optional)` and `Target` text is sent to a sentiment-classification prompt before report generation. The script asks for attitude/behaviour sentiment classifications for comment and target text.

If non-neutral sentiment contradicts the polarity implied by attitude or behaviour grades, the script writes an `ALERT`, records token usage, sets `Regenerate` true, and stops processing that row.

### Prompt constraints

The generation prompt instructs the AI to:

- write a brief yearly form-time report, not a subject-specific report;
- output only a JSON object with key `comment`;
- produce exactly four sentences;
- use attitude and behaviour descriptors accurately;
- use provided pronouns;
- avoid inventing or embellishing beyond provided information;
- include optional extra comment text exactly as given when present;
- paraphrase targets rather than copy them word-for-word;
- respect optional character-limit settings when enabled.

### Descriptor retry check

After generation, the script checks whether the final comment includes the required attitude and behaviour descriptors. It retries up to three times and writes an `ALERT` if the descriptors are still missing.

### Style-emulation controls

`AnalyseStyle.osts` limits style examples to one to five reports and performs a basic regex replacement of obvious names before sending examples to the API. Its prompt tells the AI to ignore content/topics and focus only on style.

`RunReports.osts` alerts if style emulation is enabled but no valid style profile JSON can be parsed.

### Workbook protection and state flags

Several scripts unprotect/re-protect the workbook or worksheets using `ProtectionPwd`. `RunReports.osts` uses `BusyFlag` to avoid concurrent runs.

## Pupil-name blocking

Current evidenced behaviour is limited:

- Style analysis performs basic regex name scrubbing before sending example reports to the API.
- Report generation uses a placeholder in prompts for generated output sentence starts, then replaces the placeholder with the pupil's first name after the AI response.

Important gap: the main generation workflow still uses row data that includes a pupil name for local processing, and optional free-text comments/targets may contain names or other identifiers. No robust pupil-name blocking for all free-text fields was evidenced. This should be treated as a safeguard gap.

## User-facing warning and status messages

Evidenced messages include:

- `Running…`
- `Generating your reports, please be patient...`
- `AI is now generating your report (... of ...)...`
- `Missing: Gender`, `Missing: Attitude`, `Missing: Behaviour`, or combinations thereof
- `ALERT: Comment sentiment contradicts the attitude grade.`
- `ALERT: Comment sentiment contradicts the behaviour grade.`
- `ALERT: Target sentiment contradicts the attitude grade.`
- `ALERT: Target sentiment contradicts the behaviour grade.`
- `ALERT: Style emulation enabled but no valid profile found.`
- `ALERT: AI failed to include required descriptors after 3 attempts.`
- `ERROR: ...`
- `Style emulation disabled.`
- `Enter at least one example report and rerun.`
- `Limit examples to 5 or fewer.`

Do not weaken these messages or their stop/alert behaviour without an explicit behaviour-changing task.

## Points where sensitive data could leak

- Pupil names and report-row fields in the Excel workbook.
- Optional comments and targets, especially if users paste identifiable, safeguarding, SEND, behaviour, or free-text notes.
- Style-emulation example reports.
- Generated report output cached in the workbook.
- Token/status log entries that may include IDs, timestamps, error messages, or other context.
- Hidden sheets, named ranges, comments, document properties, custom XML/script metadata, printer/page setup parts, and cached query data in workbook files.
- API keys stored in workbook named ranges.
- Local exports, temporary Excel files, or copied live workbooks.

## Current repository risks found

- The workbook template contains non-empty sample style-emulation content, style-profile output, and token-log/error entries. These were not reproduced here, but their presence should be cleaned or explicitly approved before distribution.
- The workbook contains a placeholder-like API-key value, and documentation requires that real keys must not be committed. The workbook also contains non-empty metadata fields and custom XML/script metadata that need manual Excel confirmation before distribution.
- No robust repository-level test suite currently verifies name blocking, sentiment alerts, prompt constraints, or workbook cleaning.
- No standalone prompt files make prompt review/versioning harder.
- `CancelRun.osts` references a `CancelFlag`, but a matching named range and cancellation checkpoints in `RunReports.osts` were not evidenced.

## Recommended safeguards

- Add a robust pre-AI free-text scan for names/identifiers and prohibited sensitive categories before any API call.
- Block or warn on SEND, safeguarding, medical, behaviour incident, staff names, and other high-risk terms in optional free text.
- Add a workbook pre-commit cleaning checklist and keep only a clean anonymised template in `workbook/`.
- Replace or remove sample style-emulation text, style-profile output, and token-log/error entries if the workbook should be fully blank.
- Add synthetic tests for missing fields, contradictions, name leakage, prompt output shape, and regeneration flows.
- Consider moving prompts into version-controlled `prompts/` files for safer review, while preserving existing prompt constraints.
- Keep API keys outside committed files; if workbook distribution requires a key cell, commit only a placeholder.
