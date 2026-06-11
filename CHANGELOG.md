# Changelog

All notable project changes should be recorded here.

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
