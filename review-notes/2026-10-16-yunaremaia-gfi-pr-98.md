# PR Review: yunaremaia/gfi #98 — fix(cli): print the version from gh gfi --version

**Author:** HarshRajSinghania
**Opened:** 2026-10-09 (approx)
**Status:** Merged
**Files changed:** 2 files, +N/-N lines

## What does this PR claim to do?

The problem is that `gh gfi --version` prints help text instead of the version. The `gh` extension entry point used to fold `--version` into the same code branch that handles `--help`, so the flag never reached the CLI. This PR removes `--version` from the help check and passes any unrecognized arguments through to the CLI, which then handles the flag correctly and prints `gh gfi, version 0.1.0`.

## Does the change actually accomplish that?

<!-- 3-5 sentences -->

## Edge cases, risks, or concerns

<!-- Bullet list -->

## My judgment as a maintainer

<!-- Verdict, then reasoning -->

## What I learned

<!-- 2-4 sentences -->
