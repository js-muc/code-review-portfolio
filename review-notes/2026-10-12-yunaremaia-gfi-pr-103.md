# PR Review: yunaremaia/gfi #103 — fix(cli): render issue titles and repos as literal Text to prevent Rich markup interpretation

**Author:** Fire162
**Opened:** 2026-10-10
**Status:** Merged
**Files changed:** 2 files, +45/-5 lines

## What does this PR claim to do?

This PR is a bug fix in the `gfi` CLI. When an issue title contains bracket characters — such as `[RFC]`, `[WIP]`, or `[/bold]` — Rich interprets them as markup syntax, which either crashes the command with an unhandled `MarkupError` or silently strips the brackets from the rendered output. The bug affects the `search`, `repo`, `trending`, and `feed` commands. The fix changes the shared `_truncate` helper in `src/gfi/cli.py` to return a `rich.text.Text` object instead of a plain string, which tells Rich to render the text literally rather than parsing it for markup. A regression test verifies the fix across three of the four affected commands.

## Does the change actually accomplish that?

Yes — the fix is correctly scoped and matches the stated intent. In `src/gfi/cli.py`, every place that renders `issue.title`, `issue.repo`, or `issue.labels` to a Rich table goes through the `_truncate` helper. There are exactly four such places — one in each of the `search`, `repo`, `trending`, and `feed` commands — and all four now receive a `rich.text.Text` object from the updated helper. The `--json-output` and `--csv` paths continue to use raw strings, which is correct: JSON and CSV do not interpret bracket characters as markup, so the fix is not needed there. The change is complete for the Rich rendering path, and no rendering site was missed.

## Edge cases, risks, or concerns

- **Test coverage gap for `trending`:** The PR description states that four commands are affected — `search`, `repo`, `trending`, and `feed`. The regression test covers three: `search`, `repo`, and `feed`. Since `trending` uses the same `_truncate` helper, its behavior is almost certainly fixed, but the test does not directly prove it. Small gap, not a blocker.

- **Return type change from `str` to `Text`:** `_truncate` previously returned a plain `str` and now returns a `rich.text.Text` object. Any caller that does string operations on the result — `len()`, string concatenation, `.startswith()` — could behave differently. In this codebase, all four call sites pass the result directly to `table.add_row`, which accepts both types, so the change is safe here.

- **Other user-controlled strings in the same tables:** The fix wraps `issue.title`, `issue.repo`, and `issue.labels` in `Text`. Other user-controlled strings appear in the same tables — `issue.language`, `issue.url`, and the formatted `created_at` date. In practice, these are unlikely to contain `[` or `]` characters, so the risk is low. Worth noting as a theoretical gap, not a defect in this PR.

## My judgment as a maintainer

**Verdict: Approve.** The fix is clean, correctly scoped, and addresses the root cause rather than the symptom. The change is limited to `src/gfi/cli.py`, where the shared `_truncate` helper now returns a `rich.text.Text` object, and to `tests/test_cli.py`, which adds a regression test. Every place in the CLI that renders `issue.title`, `issue.repo`, or `issue.labels` to a Rich table — one site each in `search`, `repo`, `trending`, and `feed` — goes through `_truncate`, so the fix covers all four commands. The regression test exercises three of the four (`search`, `repo`, `feed`); the fourth (`trending`) shares the same helper and is therefore almost certainly fixed. The remaining concerns — test coverage gap, the `Text` return type — are minor and do not block. I would merge as-is.

## What I learned

<!-- 1-2 sentences -->
