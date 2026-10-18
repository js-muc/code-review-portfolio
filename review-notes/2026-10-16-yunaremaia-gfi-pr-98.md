# PR Review: yunaremaia/gfi #98 — fix(cli): print the version from gh gfi --version

**Author:** HarshRajSinghania
**Opened:** 2026-10-09 (approx)
**Status:** Merged
**Files changed:** 2 files, +N/-N lines

## What does this PR claim to do?

The problem is that `gh gfi --version` prints help text instead of the version. The `gh` extension entry point used to fold `--version` into the same code branch that handles `--help`, so the flag never reached the CLI. This PR removes `--version` from the help check and passes any unrecognized arguments through to the CLI, which then handles the flag correctly and prints `gh gfi, version 0.1.0`.

## Does the change actually accomplish that?

Yes — the change accomplishes the stated intent. The diff touches two files: `src/gfi/gh_extension.py` and `tests/test_gh_extension.py`. In `src/gfi/gh_extension.py`, the change removes `--version` from the tuple of flags that trigger the help branch and adds an `else` branch that forwards any other arguments to the CLI. The help behavior for `--help`, `-h`, and empty invocation is unchanged. The fix applies to both the `gh gfi` entry point and the root `gh-gfi` script, because both call the same `main()` function. The new test in `tests/test_gh_extension.py` runs the root script directly, so the fix is verified end-to-end. No other behavior is affected.

## Edge cases, risks, or concerns

- **Unknown flags are now passed to the CLI:** Before the fix, any argument that was not `--help` or `-h` was folded into the help branch. Now the `else` branch forwards unknown flags to the CLI. This is a behavior change, but it is acceptable because the CLI has its own error handling for invalid flags.

- **No end-to-end test for `gh gfi --version`:** The new test runs `gh-gfi --version` end-to-end. The existing test for `gh gfi` uses a mock CLI — it patches `gfi.gh_extension.cli` and checks the arguments that would be passed to it, without running the real CLI. Acceptable: both entry points call the same `main()` function, so the shared logic is covered by the mock test.

- **The `else` branch handles every non-help argument, but only `--version` is tested:** Flags like `search`, `repo`, or future additions all go through the same branch. Only `--version` is explicitly tested. Acceptable for this PR, since the branch itself is small and clear.

## What I learned

<!-- 2-4 sentences -->
