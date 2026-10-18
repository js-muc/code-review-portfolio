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
  
## My judgment as a maintainer

**Verdict: Approve.** The fix is small, precise, and addresses the root cause. It removes `--version` from the help check and forwards all other arguments to the CLI. The change is limited to `src/gfi/gh_extension.py` and `tests/test_gh_extension.py`. Both the `gh gfi` entry point and the root `gh-gfi` script are covered — one by a mock test, the other by an end-to-end test. The concerns I listed — unknown flags reaching the CLI, no end-to-end test for `gh gfi --version`, and the new `else` branch handling every non-help argument — are minor and acceptable for this change. The help behavior for `--help`, `-h`, and empty invocation is unchanged. I would merge as-is.

## What I learned

This PR taught me the difference between a symptom and a cause. In my first draft of the description, I wrote "the code treats `--version` as `--help`." That is the symptom — what the user sees. The actual cause is the line of code that groups `--version` with `--help` in the same check. A good bug description names both. A good fix targets the cause, not the symptom. I will look for this distinction in every PR I review from now on.
