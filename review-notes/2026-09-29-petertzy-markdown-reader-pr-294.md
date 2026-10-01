# PR Review: petertzy/markdown-reader #294 — docs: update Node.js requirement in README to 20.9.0+

**Author:** js-muc
**Opened:** 2026-09-29
**Status:** Open
**Files changed:** 1 file, +1/-1 lines

## What does this PR claim to do?

This PR corrected a document inconsistency. The README stated that the project requires Node.js 18+, but `CONTRIBUTING.md` and `frontend/package.json` both specified Node.js 20.9.0+. A new contributor following the `README.MD` would install Node.js 18.x and encounter an environmental mismatch during setup, with no clear indication of the cause. This PR aligned the README with the project's actual requirement.

## Does the change actually accomplish that?

Yes — the change accomplishes the stated intent. The diff shows exactly one line changed in `README.MD`: `Node.js 18+` was replaced with `Node.js 20.9.0+`. No other files were modified. This aligns the README with the requirement declared in `frontend/package.json` (`engines.node: ">=20.9.0"`) and already documented in `CONTRIBUTING.md`.

## Edge cases, risks, or concerns

None that I can identify. This is a documentation-only change to `README.MD`, so no code, tests, build configuration, or runtime behavior is affected. The new value (`Node.js 20.9.0+`) matches the requirement declared in `frontend/package.json` (`engines.node: ">=20.9.0"`) and the value already stated in `CONTRIBUTING.md`, so the three files are now consistent.

One open question: whether the previous value (`18+`) was correct at an earlier point in the project's history. Checking the git history of `frontend/package.json` and `CONTRIBUTING.md` would confirm whether the requirement changed over time. This does not affect the correctness of this PR — the new value is correct regardless — but it may be useful context for future maintainers.

## My judgment as a maintainer

**Verdict: Approve.** This is a documentation-only change with no effect on code behavior, build, or runtime. It aligns `README.MD` with the Node.js requirement already declared in `frontend/package.json` and stated in `CONTRIBUTING.md`. The change is minimal, verified against two independent sources, and introduces no risk. I would merge it as-is.

## What I learned

Spotting the bug was easy, but communicating the fix professionally was harder than I expected. The maintainer reviewed and merged the PR quickly, and I suspect that is because the PR was well documented: the description explained what changed, why it mattered, and how the change was verified. The takeaway is that a properly documented PR saves the maintainer time and tends to get a faster response.
