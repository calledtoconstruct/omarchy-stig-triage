# Guiding principles: calledtoconstruct/omarchy-stig-triage

## Purpose and scope
Triage notes mapping the Ubuntu 24.04 STIG V1R6 workbook onto Omarchy, current image and a future Omarchy-owned base.

## Stack and layout
`Omarchy_STIG_Triage_UBTU24_V1R6.xlsx` and `PLAN.md`. No code.

## Commands
| Check | Command | Evidence |
|---|---|---|
| Build / type-check / tests | Not applicable: a spreadsheet and a plan | repository tree |
| CI | None | repository tree |

Commands marked as not written down come from the build files, not from project docs; confirm them before relying on them.

## Before every push
Before every push: run the build and type-check/lint that exist here, plus only the tests for the files or feature you changed (team rule, ideas #14 note 6797). Do not run the full suite locally as a routine gate; this repository has no CI that runs the full suite, so run the full suite only when the change is broad, and say in the PR that CI coverage is missing. Merge needs a positive review. Explain any new failure your change caused. Never weaken a valid test to make it pass, and never invent a command this file does not list.

## Project rules
- Follow PLAN.md: SRG-OS IDs are the join key; do not mint STIG IDs or call the result a STIG.
- Each 'already true' note says whether it holds for the current image only or also once Omarchy owns its kernel and repositories.
- A fix that would change every public install is not an acceptable fix entry.

## What to avoid
- Committing to the default branch; work on a branch and open a pull request.
- Deleting files, branches or history, or changing repository settings.
- Adding tooling, CI or dependencies that the change does not need.
- Rules copied from other projects (file-size limits, clocks, quotas) that this repository never adopted.

## Delivery and safety
- Nothing is published.

## Open questions
- Spreadsheet changes cannot be reviewed as diffs; describe changed rows in the PR.

## Sources
Read from the `main` branch on 2026-11-08 ET: `PLAN.md`, repository tree.

## Instruction updates
Check `guiding-principles.version` at the start of each turn and when you enter this project; reload this file when the version changes. Change this file, its version and date, and `PRINCIPLES_HISTORY.md` in the same commit. Team-wide role instructions are versioned separately in the team-process repository.
