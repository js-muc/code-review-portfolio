# PR Review: peterzty/markdown-reader #294 — docs: update Node.js requirement in README to 20.9.0+

**Author:** js-muc
**Opened:** 2026-09-29
**Status:** Open
**Files changed:** 1 file, +1/-1 lines

## What does this PR claim to do?

This PR corrected a document inconsistency. The README stated that the project requires Node.js 18+, but `CONTRIBUTING.md` and `frontend/package.json` both specified Node.js 20.9.0+. A new contributor following the `README.MD` would install Node.js 18.x and encounter an environmental mismatch during setup, with no clear indication of the cause. This PR aligned the README with the project's actual requirement.

## Does the change actually accomplish that?

<!-- Write your assessment here -->

## Edge cases, risks, or concerns

<!-- List any issues, or state clearly that there are none -->

## My judgment as a maintainer

<!-- Approve / Request changes / Comment, then explain why -->

## What I learned

<!-- 1-2 sentences -->
