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

- **Behavior change in `pypdf` 6.19.0:** A version bump from `6.16.1` to `6.19.0` is more than a patch update. If the new version changed any public API that CrewAI's PDF handling relies on, that could surface as a runtime error. The author claims the change is safe because PDF handling uses the standard `PdfReader`/`PdfWriter` APIs, and 18 PDF tests pass. The claim is plausible but not verified from the diff alone.

- **Downstream consumers with pinned old versions:** Any project that depends on CrewAI and pins `pypdf` at `6.16.1` in its own lockfile will now face a dependency resolution conflict against this PR's `>=6.19.0` floor. This is not a bug — it is how dependency bumps work — but it is a real consequence for downstream users and may require them to update their own locks.

- **No new test added:** The PR description checks "Tests added or updated for the changed behavior," but what actually happened is that existing tests were run (18 PDF tests passed). No new test confirms the security fix specifically. For a dependency bump, existing coverage is arguably sufficient, but the label is slightly imprecise.

## My judgment as a maintainer

**Verdict: Approve.** This is a security-driven dependency bump with no application code changes, so the risk to CrewAI's own runtime behavior is minimal. The three files agree on the new `pypdf` floor (`>=6.19.0`), the lockfile resolves consistently, and all 18 PDF tests pass — which specifically exercises the code paths affected by the change. The remaining risks — a behavior change inside `pypdf` itself, and downstream conflicts for consumers who pin the old version — are inherent to any dependency bump and are acceptable here. I would merge it as-is.

## What I learned

<!-- 1-2 sentences -->
