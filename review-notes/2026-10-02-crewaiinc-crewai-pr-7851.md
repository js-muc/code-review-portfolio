# PR Review: crewAIInc/crewAI #7851 — fix(deps): bump pypdf to 6.19.0 to clear pip-audit findings

**Author:** Vidit-Ostwal
**Opened:** 2026-10-01
**Status:** Merged
**Files changed:** 3 files, +17/-10 lines

## What does this PR claim to do?

This PR is a security-driven dependency bump. It raises the `pypdf` floor from `>=6.16.1` to `>=6.19.0` across both `lib/crewai-files/pyproject.toml` and the workspace override in `pyproject.toml`, with a matching `uv.lock` update. The change closes eight document-parsing advisories (memory and denial-of-service issues) that were reported against `pypdf < 6.19.0`. The author verified the fix with a passing `pip-audit` run and an OSV query that reports no vulnerabilities for `pypdf 6.19.0`. No application code is modified.

## Does the change actually accomplish that?

Yes — the diff matches the stated intent. In `lib/crewai-files/pyproject.toml`, the single line `"pypdf~=6.16.1",` was changed to `"pypdf~=6.19.0",`. In the root `pyproject.toml`, the override entry was updated from `"pypdf>=6.16.1,<7",` to `"pypdf>=6.19.0,<7",`, and the advisory comment block was expanded to document the advisories closed by the new floor. `uv.lock` resolves `pypdf` at `6.19.0`. No application code or unrelated files were modified. The three files agree on the new floor, and the change is consistent with the security fix described by the author.

## Edge cases, risks, or concerns

<!-- What could go wrong? -->

## My judgment as a maintainer

<!-- Verdict, then reasoning -->

## What I learned

<!-- 1-2 sentences -->
