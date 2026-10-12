# PR Review: yunaremaia/gfi #103 — fix(cli): render issue titles and repos as literal Text to prevent Rich markup interpretation

**Author:** Fire162
**Opened:** 2026-10-10
**Status:** Merged
**Files changed:** 2 files, +45/-5 lines

## What does this PR claim to do?

This PR is a bug fix in the `gfi` CLI. When an issue title contains bracket characters — such as `[RFC]`, `[WIP]`, or `[/bold]` — Rich interprets them as markup syntax, which either crashes the command with an unhandled `MarkupError` or silently strips the brackets from the rendered output. The bug affects the `search`, `repo`, `trending`, and `feed` commands. The fix changes the shared `_truncate` helper in `src/gfi/cli.py` to return a `rich.text.Text` object instead of a plain string, which tells Rich to render the text literally rather than parsing it for markup. A regression test verifies the fix across three of the four affected commands.

## Does the change actually accomplish that?

<!-- 3-5 sentences -->

## Edge cases, risks, or concerns

<!-- Bullet list -->

## My judgment as a maintainer

<!-- Verdict, then reasoning -->

## What I learned

<!-- 1-2 sentences -->
