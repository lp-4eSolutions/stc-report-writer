# Archive note

Date: 2026-06-12

## Purpose of this trial repository

This repository was used as a trial run for a ChatGPT + Codex + GitHub workflow around an Excel/Office Scripts form-tutor report writer concept. It is being mothballed as an archive/reference repository, not continued as the active production report writer project.

## Workflow tested

The trial exercised a repository-maintenance workflow that included:

- repository-specific agent instructions in `AGENTS.md`;
- evidence-based project-state tracking in `PROJECT_STATE.md`;
- decision and changelog discipline in `DECISIONS.md` and `CHANGELOG.md`;
- Codex-assisted documentation audits and pull-request-sized changes;
- workbook-hygiene documentation for an `.xlsx` template where binary workbook changes were not suitable for the PR/review path;
- Git commit and PR review flow before manual merge/archive decisions.

## What worked well

- `AGENTS.md` gave future agents clear safety rules, start-of-task reading, and documentation-maintenance expectations.
- `PROJECT_STATE.md` provided a useful place to separate evidenced facts, unknowns, and next steps.
- The docs folder became a readable reference for workflow, workbook setup, safeguards, testing, and workbook hygiene.
- The trial showed that documentation-only PRs can capture important workbook risks even when binary workbook edits cannot be reviewed safely.
- Commit and changelog entries made the sequence of trial tasks easier to reconstruct.

## Known limitations

- This repository is not production-ready and is not the active production project.
- The checked-in workbook has not been proven to be a clean blank template. Existing workbook-hygiene documentation records non-empty style-emulation examples, style-profile output, token/error log rows, and metadata requiring manual Excel confirmation.
- No automated test harness, standalone Power Query source, standalone prompt files, or synthetic fixture suite is evidenced in the repository.
- Exact Office Script assignments for workbook buttons/shapes and some workbook metadata cannot be fully verified from repository text alone.
- A text and documented-audit safety scan did not identify obvious real API keys or live school-data files, but that is not a guarantee that workbook package contents are safe for redistribution.

## Data-safety reminder

Do not use this repository with real pupil, staff, SEND, safeguarding, behaviour, assessment, report, target, comment, generated-report, or other sensitive school data. Do not add real API keys, credentials, private endpoints, live school exports, or working workbooks. If any sensitive material is discovered later, do not reproduce it in issues or documentation; record the risk type only and follow the organisation's incident/remediation process.

## How to restart work later

Recommended approach: start a fresh repository for the real project, then deliberately copy only selected safe patterns or documents after review. Potentially useful material to carry forward includes:

- the `AGENTS.md` structure and safety expectations;
- the `PROJECT_STATE.md` evidence/uncertainty format;
- `docs/testing-checklist.md`;
- `docs/workbook-hygiene-audit.md` as an audit-template example;
- lessons from the Git + Codex + PR workflow captured in the changelog and project-state notes.

If this repository must be reused instead, explicitly unarchive or fork it first, open a new project-state section, re-audit the workbook and scripts, clean or replace the workbook through an approved binary-review route, and confirm data-protection requirements before any production work.

## Archive recommendation

After this documentation-only archive PR is reviewed and merged, manually archive the repository in GitHub. Leave the repository readable as a workflow-trial reference unless a future owner deliberately forks/unarchives it and performs a fresh safety review.
