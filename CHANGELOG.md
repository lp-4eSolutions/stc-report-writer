# Changelog

All notable project changes should be recorded here.

## 2026-06-11 - PR-safe workbook cleaning remediation

- Removed the workbook binary modification from the pull-request diff because the review/PR path reported that binary files are not supported.
- Kept the workbook-cleaning requirements as documentation so the template residue can be cleared manually in Excel or by a local package-level process outside the unsupported binary PR path.
- No Office Script, Power Query, prompt, config, workbook binary, formula, named-range, worksheet, protection, shape, button, or report-generation logic is changed by this remediation.

## 2026-06-11 - Workbook hygiene audit documentation

- Added a workbook hygiene audit report for `workbook/Form Tutor Report Writer (BLANK).xlsx`.
- Documented non-empty workbook template residue, including style-emulation examples, style-profile output, and token/error log rows, without reproducing sensitive values.
- Strengthened workbook/data-protection documentation around metadata, custom XML/script metadata, and pre-commit workbook checks.
- No Office Script, Power Query, prompt, config, formula, named-range, worksheet, protection, shape, button, or workbook behaviour was changed.

## 2026-06-11 - Documentation and `.gitignore` refinement

- Refined `.gitignore` CSV rules so generic CSV/TSV exports remain ignored while clearly named synthetic/example/template CSV fixtures and config example/template CSVs can be committed.
- Clarified the synthetic CSV fixture naming rule in the testing checklist.
- No Office Script, Power Query, prompt, config, or workbook behaviour was changed.

## 2026-06-11 - Documentation audit and source-of-truth pass

- Populated project documentation from repository evidence only.
- Documented the Office Script workflow, workbook setup, current safeguards, workbook risks, testing checklist, and future-agent instructions.
- Added ignore rules to reduce the risk of committing live school data, Excel temporary files, local exports, API keys, and credentials.
- No Office Script, Power Query, prompt, config, or workbook behaviour was changed.
