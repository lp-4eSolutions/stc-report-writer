# Workbook hygiene audit

Audit date: 2026-06-11

Workbook inspected: `workbook/Form Tutor Report Writer (BLANK).xlsx`

## Scope and method

This was a workbook hygiene audit and documentation update only. No workbook content, formulas, names, worksheet structure, shapes, protection, Office Scripts, Power Query files, prompts, or config behaviour were changed.

Inspection was performed outside Excel by treating the `.xlsx` file as an Office Open XML package and reading workbook XML/package parts with local scripts. The audit checked, as far as practical without opening Excel:

- workbook worksheet names and sheet visibility states;
- workbook-level and sheet-level defined names;
- non-empty cell coordinates/counts by worksheet and value type;
- formula locations;
- data-validation ranges and validation types;
- table, query, connection, external-link, pivot, comment, and threaded-comment package parts;
- drawing/shape names;
- document property part names and non-empty metadata fields;
- custom XML part presence;
- package-level text patterns that look like API keys, local paths, private endpoints, email addresses, or high-risk school-data terms.

Sensitive values were not copied into this report. Locations below identify only the worksheet/range and the category of content or risk.

## Worksheets found

All workbook sheets found in `xl/workbook.xml` were marked `visible`; no hidden or very-hidden sheets were evidenced in the package inspection.

| Worksheet | Visibility evidenced in XML | Non-empty bounds evidenced in XML | Notes |
| --- | --- | --- | --- |
| `Settings` | Visible | `A1:F12` | Contains settings labels/values, style-emulation example cells, style-profile output cells, one formula, data validation on `C1`, sheet protection, and drawing parts. |
| `Reports` | Visible | `A1:CD503` | Contains row-one report headers, formulas in `A4:A503`, regeneration flag values in `Q4:Q503`, the `BusyFlag` value at `CD102`, data validation, sheet protection, and drawing parts. The clearable report data range `ReportsData` was empty. |
| `Lists` | Visible | `A1:B15` | Contains validation/support list values. |
| `TokenLog` | Visible | `A1:D5` | Contains non-empty log/header content and four non-empty log rows. |
| `Config` | Visible | `A1:H24` | Contains configuration labels/values, formula cells, and named-range-backed configuration values. |

## Hidden or very-hidden sheets

No hidden or very-hidden worksheet state was evidenced in the workbook XML. This should still be confirmed manually in Excel because workbook UI state and add-in/script metadata are easier to inspect there.

## Named ranges found

All defined names found were workbook-scoped. No sheet-scoped defined names were evidenced.

| Defined name | Refers to | Hygiene note |
| --- | --- | --- |
| `BusyFlag` | `Reports!$CD$102` | Non-empty runtime flag cell. Preserve the named range and expected type if cleaning. |
| `CharLimit` | `Settings!$C$1` | Non-empty setting cell. |
| `CharLimitFlag` | `Settings!$B$1` | Non-empty setting cell. |
| `ContactEmail` | `Config!$H$1` | Formula-backed contact field; package scanning found an email-address pattern here. Confirm this is a generic project/support contact before distribution. |
| `EnableEmulation` | `Settings!$B$3` | Non-empty setting cell. |
| `ExtraCurricList` | `Lists!$A$2:$A$15` | Non-empty validation/support list. |
| `FreqPenalty` | `Config!$A$4` | Non-empty model parameter. |
| `Model` | `Config!$A$2` | Non-empty model parameter. |
| `OpenAI_Key` | `Config!$A$1` | Non-empty placeholder-like value; no API-key-like pattern was detected in the XML. Do not commit a real key. |
| `PresPenalty` | `Config!$A$5` | Non-empty model parameter. |
| `ProtectionPwd` | `Config!$A$17` | Non-empty protection password/config value. Confirm it is suitable for a template and not a reused private credential. |
| `RegenAll` | `Reports!$Q$1` | Non-empty header/control cell. |
| `RegenFlags` | `Reports!$Q$4:$Q$503` | Contains 500 non-empty flag values. Preserve the range and expected boolean/default values if cleaning. |
| `ReportsData` | `Reports!$B$4:$P$503` | Empty in the package inspection. |
| `RunReportsDefaults` | `Config!$A$24` | Non-empty JSON/config snapshot for the run button. Preserve if cleaning unless the button defaults are intentionally regenerated. |
| `StyleExamples` | `Settings!$B$4:$F$4` | Contains five non-empty style-emulation example cells. These should be cleared or replaced with explicitly synthetic placeholders before treating the workbook as a blank template. |
| `StyleProfileJSON` | `Settings!$F$3` | Non-empty style-profile output cell. Clear before treating the workbook as a blank template unless a generic built-in style profile is intentionally retained. |
| `StyleProfileText` | `Settings!$D$3` | Non-empty style-summary output cell. Clear before treating the workbook as a blank template unless a generic built-in style profile is intentionally retained. |
| `Temperature` | `Config!$A$3` | Non-empty model parameter. |
| `TokenCol` | `Reports!$P$4:$P$503` | Empty in the package inspection. |
| `Version` | `Config!$A$6` | Formula-backed/non-empty version cell. |

## Non-empty areas found

The package inspection found these non-empty areas. This section intentionally describes content categories rather than workbook values.

| Worksheet | Range/area | Category |
| --- | --- | --- |
| `Settings` | `A1:F12` | Settings labels, values, one formula, style-emulation examples, style-summary/profile output, and UI/help text. |
| `Settings` | `B4:F4` | Five non-empty style-emulation example cells. |
| `Settings` | `D3` and `F3` | Non-empty style-profile output cells. |
| `Reports` | `A1:Q1` with spacer columns | Report input/output headers and regeneration header/control cell. |
| `Reports` | `A4:A503` | 500 formula cells used for generated/structured IDs. |
| `Reports` | `Q4:Q503` | 500 non-empty regeneration flag values. |
| `Reports` | `CD102` | Non-empty busy/runtime flag cell. |
| `Reports` | `B4:P503` | No non-empty report data found in the named `ReportsData` range. |
| `Lists` | `A1:B15` | Validation/support list content. |
| `TokenLog` | `A1:D5` | Non-empty token/error log headers and four non-empty log rows. |
| `Config` | `A1:A6`, `A11:A13`, `A17`, `A24`, `D1:G20`, `H1` | Config values, labels/help text, formulas, contact field, protection/config value, and run-button defaults. |

## API key, credentials, endpoints, and local paths

- The `OpenAI_Key` named range points to `Config!$A$1` and contains a non-empty placeholder-like value. No API-key-like pattern was detected for that cell in the XML inspection.
- No API-key-like values were detected by the package-level text pattern scan.
- No local file paths or private endpoint URL patterns were detected by the package-level text pattern scan.
- `ProtectionPwd` is non-empty. It should be treated as workbook configuration/protection data, and the project owner should confirm it is not reused from any real system or private credential.
- `ContactEmail` contains an email-address pattern in a formula-backed cell. Confirm it is a generic project/support address before distribution.

## Token-log and error-log content

`TokenLog` contains non-empty content in `A1:D5`, including four non-empty rows below the header area. The audit did not reproduce those values. Even if these entries are synthetic or old test residue, they should be cleared before the workbook is described as a blank template.

## Style-emulation examples and profile output

The style-emulation named range `StyleExamples` contains five non-empty cells, and both `StyleProfileText` and `StyleProfileJSON` are non-empty. The audit did not reproduce any of this text. These cells should be cleared before the workbook is described as a blank template unless the project deliberately ships with a generic, synthetic style profile.

## Comments, notes, tables, queries, and connections

The package inspection found:

- no comments or threaded-comments parts;
- no Excel table parts;
- no workbook connection parts;
- no query parts;
- no external-link parts;
- no pivot parts;
- no VBA project parts.

The repository `power-query/` folder also contains no exported Power Query source. Power Query behaviour remains `Not evidenced in repo`.

## Formulas and data validation

Formula cells were found at:

- `Settings!E2`;
- `Reports!A4:A503`;
- `Config!H1`, `Config!A6`, `Config!A11`, and `Config!A12`.

Data-validation definitions were found on:

- `Settings!C1`;
- `Reports!C4:C1048576`;
- `Reports!C1:H3`;
- `Reports!D504:F1048576`;
- `Reports!F4:F1048576`;
- `Reports!G9:H503 G4:G8`.

Any workbook cleaning must preserve these formulas and validation definitions unless a separate behaviour-changing task explicitly changes workbook functionality.

## Drawing and shape names

Drawing XML contained these shape names:

- `RunReports`;
- `SelectCompletedButton`;
- `AnalyseStyleButton`;
- `Rounded Rectangle 1`;
- `Rounded Rectangle 3`;
- `Rounded Rectangle 4`;
- `Rounded Rectangle 5`;
- `Picture 3`.

The `RunReports` shape name is script-relevant. Script/button assignments cannot be confirmed reliably from XML alone and need manual Excel confirmation.

## Document properties and metadata

Document property parts were present and had non-empty metadata fields, including core creator/last-modified fields, created/modified timestamps, application fields, workbook part titles, and a custom property. The audit did not reproduce metadata values.

No API-key-like, local-path, private-endpoint, or high-risk school-data pattern was detected in document property text. However, because document properties can contain personal names or organisation-specific details, the metadata should be manually checked and cleared or normalised before distribution.

## Custom XML and binary parts

The package contains custom XML parts, including a script-ID-related custom XML part, and two binary printer-settings parts. No API-key-like, local-path, or private-endpoint pattern was detected in the custom XML text scan, but these parts were not semantically validated.

Manual Excel checking is recommended for:

- Office Script/button assignments;
- custom XML/script metadata;
- printer/page setup metadata;
- document properties shown in Excel's file information UI.

## Sensitive-data and credential risk assessment

The audit did not detect package-level text patterns that looked like real API keys, local file paths, private endpoints, SEND/safeguarding/behaviour/assessment terms, or obvious live school-data categories. The audit also found that `ReportsData` and `TokenCol` were empty.

However, the workbook is not currently blank because it contains:

- non-empty style-emulation examples;
- non-empty style-profile output;
- non-empty token/error log rows;
- non-empty configuration/protection/contact metadata;
- document metadata that requires manual confirmation.

Because sensitive values must not be reproduced, the absence of a detected pattern is not a guarantee that every string is safe. Manual Excel review remains required before public redistribution.

## Recommended cleaning actions

Recommended follow-up cleaning task, if the project owner wants this workbook to be a safe blank template:

1. Clear `Settings!B4:F4`.
   - Content to clear: style-emulation example text.
   - Preserve: the `StyleExamples` named range, formatting, validation/protection, row/column layout, and any button/script compatibility.
2. Clear `Settings!D3` and `Settings!F3`.
   - Content to clear: generated style summary/profile output.
   - Preserve: the `StyleProfileText` and `StyleProfileJSON` named ranges, formatting, validation/protection, and layout.
3. Clear old log entries from `TokenLog` while preserving the header structure expected by scripts.
   - Content to clear: rows below the header that contain prior token/error/status entries.
   - Preserve: worksheet name, columns/header row, formatting, and any script expectations for appending logs.
4. Confirm whether `Config!A17` and `Config!H1` are acceptable template values.
   - Content to review: protection/config value and formula-backed contact field.
   - Preserve: `ProtectionPwd` and `ContactEmail` named ranges and formula/functionality if retained.
5. Clear or normalise document properties in Excel if they contain personal or organisation-specific metadata.
6. Reopen the cleaned workbook in Excel and verify shape/button script assignments, protection, validation drop-downs, formulas, and Office Script workflows still work.

## PR-safe remediation after binary PR rejection - 2026-06-11

A package-level workbook cleaning pass was attempted for `workbook/Form Tutor Report Writer (BLANK).xlsx`, but the resulting `.xlsx` change was removed from the pull-request diff because the review/PR path reported that binary files are not supported. This documentation update therefore records the required cleanup and validation without changing the workbook binary in this PR.

Required workbook cleanup remains:

- Clear `Settings!B4:F4` while preserving the `StyleExamples` named range, formatting, validation, protection, row/column layout, and worksheet structure.
- Clear `Settings!D3` and `Settings!F3` while preserving the `StyleProfileText` and `StyleProfileJSON` named ranges, formatting, validation, protection, row/column layout, and worksheet structure.
- Clear all old `TokenLog` rows below the header while preserving the `TokenLog` worksheet and the header/column structure expected by `office-scripts/RunReports.osts`.
- Rebuild or compact shared strings, and inspect comments, custom XML, document properties, and other package text so cleared text is not retained in package parts.
- Normalise personal or organisation-specific document properties only where this can be done safely without damaging workbook compatibility; otherwise leave them for manual Excel review.

Any future cleaning route should still verify that worksheet names and visibility states, defined names/named ranges, formulas, data-validation ranges, protection XML, drawing/shape names, package parts, `ReportsData`, and `TokenCol` remain unchanged, and that no API-key-like values, local paths, private endpoints, obvious high-risk school-data terms, or cleared residue strings remain in package text.

Final recommendation for this PR-safe remediation: keep this PR documentation-only and perform the workbook binary cleanup through a route that supports `.xlsx` review/storage, or clean the workbook manually in Excel and attach/distribute it through an approved binary artifact process. Until that happens, the checked-in workbook should still be treated as needing the specific cleaning listed above before it is used as a blank template.

## Workbook parts that cannot be checked reliably outside Excel

The following require manual confirmation in Excel or with Microsoft 365 tooling:

- exact Office Script assignments on buttons/shapes;
- whether custom XML script IDs map to the intended scripts in the user's tenant;
- very detailed page setup/printer settings stored in binary printer-settings parts;
- document properties as presented by Excel and Microsoft 365;
- whether any add-ins, workbook links, or tenant-specific automation metadata are surfaced only in the Excel UI;
- whether all formulas recalculate as expected after opening the workbook;
- whether worksheet protection and allowed-edit behaviour match the intended user workflow.

## Final recommendation

Keep after specific cleaning.

The workbook does not show obvious API-key-like values, local paths, private endpoints, hidden sheets, table/query/connection parts, or populated report-row data in the inspected package. It should not yet be treated as a safe blank template because non-empty style-emulation examples, style-profile output, and token/error log rows remain present, and document/custom metadata still needs manual Excel confirmation.
