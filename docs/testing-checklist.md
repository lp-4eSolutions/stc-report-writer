# Testing checklist

Use synthetic or anonymised data only. Never test with real pupil, staff, SEND, safeguarding, behaviour, assessment, report, or free-text data in a committed workbook.

## Before testing

- [ ] Work from a copy of the template workbook, not the repository template itself.
- [ ] Confirm `OpenAI_Key` contains a valid local key only in the working copy.
- [ ] Confirm the working copy will not be committed.
- [ ] Confirm all test pupils, comments, and targets are synthetic.
- [ ] Commit CSV fixtures only when clearly named `synthetic-*.csv`, `example-*.csv`, or `template-*.csv` under `test-data/`; live/exported school data remains ignored and must not be committed.
- [ ] Confirm Office Scripts are assigned to the intended workbook buttons.
- [ ] Confirm `ProtectionPwd` works for scripts that unprotect/re-protect.

## Synthetic test scenarios

### Basic generation

- [ ] One row with valid `ID`, synthetic `Name`, `Gender`, `Attitude`, `Behaviour`, optional extracurricular value, no comment, and no target.
- [ ] Expected: four-sentence report, `Complete` status, token count, `Regenerate` false, log entry.

### Missing required fields

- [ ] Missing `Gender`.
- [ ] Missing `Attitude`.
- [ ] Missing `Behaviour`.
- [ ] Missing multiple required fields.
- [ ] Expected: `Missing: ...` status, highlighted cells, no generated report, no report-generation API call for that row.

### Off-site row

- [ ] `Off Site?` set to `YES` with otherwise valid row.
- [ ] Expected: local non-AI attendance sentence, `Complete`, zero tokens, `Regenerate` false.

### Regeneration behaviour

- [ ] Completed row with `Regenerate` false.
- [ ] Completed row with `Regenerate` true.
- [ ] Run `ToggleRegenAll.osts` with completed rows.
- [ ] Expected: completed unflagged rows are skipped; flagged rows regenerate; toggle script flips eligible complete/report rows.

### Optional comments and targets

- [ ] Positive comment with positive grades.
- [ ] Negative target with low grades.
- [ ] Comment that contradicts attitude grade.
- [ ] Comment that contradicts behaviour grade.
- [ ] Target that contradicts attitude grade.
- [ ] Target that contradicts behaviour grade.
- [ ] Expected: aligned free text proceeds; contradictory free text produces the relevant `ALERT` and keeps regeneration enabled.

### Character limit

- [ ] Character limit disabled.
- [ ] Character limit enabled at an allowed value.
- [ ] Expected: prompt includes hard character-limit instructions only when enabled; outputs should be manually checked because no post-generation hard enforcement was evidenced beyond prompt instruction.

### Style emulation

- [ ] Emulation disabled.
- [ ] Emulation enabled with no examples.
- [ ] Emulation enabled with one synthetic example.
- [ ] Emulation enabled with five synthetic examples.
- [ ] Emulation enabled with more than five examples.
- [ ] Invalid/missing `StyleProfileJSON` while `EnableEmulation` is true.
- [ ] Expected: documented messages/alerts; valid profile used only when parseable.

### API and error handling

- [ ] Invalid API key in a non-committed working copy.
- [ ] Network/API failure.
- [ ] Malformed AI JSON response if this can be simulated.
- [ ] Expected: `ERROR: ...` status or script error path without exposing sensitive data in committed files.

## Workbook tests

- [ ] Required worksheets exist: `Settings`, `Reports`, `Lists`, `TokenLog`, `Config`.
- [ ] Required named ranges exist and refer to expected cells/ranges.
- [ ] Required `Reports` headers are present and unique.
- [ ] Data validation dropdowns work in the intended input cells.
- [ ] Protection allows intended user edits but prevents accidental structural edits.
- [ ] Buttons/shapes trigger the expected Office Scripts.
- [ ] `ClearAll.osts` clears values but preserves formatting and validation.
- [ ] `ResetReports.osts` restores the `RunReports` button and clears `BusyFlag`.
- [ ] `SettingsButton.osts` and `ReturnToReports.osts` navigate and hide/show `Settings` as intended.

## Office Script checks

- [ ] Scripts handle an empty `Reports` data area gracefully.
- [ ] Scripts handle missing required named ranges with clear errors.
- [ ] Header-driven scripts fail clearly if a required header is missing.
- [ ] `BusyFlag` prevents overlapping runs.
- [ ] Token logs append to `TokenLog` as expected.
- [ ] Sheet/workbook protection is restored after successful runs.
- [ ] Failure paths do not leave the workbook permanently unprotected or `BusyFlag` stuck true; use `ResetReports.osts` as a recovery check.

## Power Query refresh checks

Power Query behaviour is currently not evidenced in the repository. If Power Query is later added:

- [ ] Export query source into `power-query/`.
- [ ] Document source columns and transformations.
- [ ] Refresh queries using synthetic data.
- [ ] Confirm refresh does not cache real school data in committed workbooks.
- [ ] Confirm column-renaming or missing-column behaviour is documented.

## AI prompt/output checks

- [ ] Output is valid JSON containing only `comment`.
- [ ] Comment is exactly four sentences.
- [ ] Comment includes required attitude and behaviour descriptors.
- [ ] Comment uses provided pronouns correctly.
- [ ] Comment does not invent unsupported facts.
- [ ] Target is paraphrased rather than copied word-for-word.
- [ ] Optional comment handling is reviewed carefully because the prompt currently says to include extra comment text exactly as given.

## Safeguarding/blocking tests

These are recommended gaps to test once implemented:

- [ ] Pupil-name/identifier appears in optional free text and is blocked or warned before API calls.
- [ ] Staff name appears in optional free text and is blocked or warned.
- [ ] SEND/safeguarding/medical/behaviour incident terms appear and are blocked or warned.
- [ ] Realistic accidental pasted paragraph is blocked before leaving the workbook.
- [ ] Style examples containing names are scrubbed or blocked robustly.

## Regression checks before using with real school workflows

- [ ] Documentation is up to date with current scripts and workbook structure.
- [ ] Template workbook has been manually inspected for hidden/cached data.
- [ ] No real data has been committed.
- [ ] `.gitignore` rules are in place for local exports and live data folders.
- [ ] A synthetic end-to-end run has been completed successfully.
- [ ] A synthetic contradiction/alert run has been completed successfully.
- [ ] A blank/clear/reset workflow has been completed successfully.
