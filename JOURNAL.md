## Week 7 — Issue selection

**Issue link:** [(https://github.com/ascherj/pathreview/issues/146)]

**Issue title:** PII scrubber fails to redact parenthesized US phone numbers

**Tier:** [x] Tier 1  [ ] Tier 2  [ ] Tier 3

**Problem summary:**
The PII scrubber currently redacts dashed US phone numbers like 555-123-4567 but misses the common parenthesized format (555) 123-4567. Because of this pattern gap in safety/pii_scrubber.py, scrub() can leave sensitive phone numbers unredacted and detect() can return no PII for the same input. This creates a privacy risk and causes related unit tests to fail for several US phone formatting cases. A successful fix will update phone matching so parenthesized US numbers are consistently detected and redacted, and the existing failing tests pass.

**Branch name:** [fix/146-PII-scrubber-redact-parenthesized-US-phone-numbers]

**Setup confirmation:** [x] App runs locally at localhost:5173

**Cohort ledger:** [ ] Issue added to cohort ledger

**Issue claim reasoning:**
I confirmed the affected file path (tests/unit/test_pii_scrubber.py) and understand the expected before/after behavior.
I've estimated the time this will take and I'm confident I can complete it before the Week 9 deadline.

## Week 8 — Reproduction & solution planning

**Reproduction commit link:** [(https://github.com/coderYL2337/pathreview/commit/41b0da2abaebc1a3c12078da033bfdd660638756)]

**Reproduction summary:**
I reproduced issue #146 by running PIIScrubber with both (555) 123-4567 and 555-123-4567. scrub() redacted only the dashed number and detect() returned an empty list for the parenthesized number, confirming the bug.

**PLAN.md link:** [PLAN.md](PLAN.md)

**Walkthrough video (recommended):** [link to your Loom video, ≤2 min — shared for early feedback]

**Blockers or open questions:**
[Anything you're still uncertain about going into Week 9, or leave blank]

## Week 9 — Solution building & PR submission

### Check-in 1 (mid-week)

**Current progress:**
Implemented the phone_us regex fix in `safety/pii_scrubber.py`: replaced the leading `\b` with a `(?<!\w)` lookbehind so the match can include the opening `(`, and added `\s` alongside `-`/`.` as valid separators so parenthesized and space-separated formats (e.g. `(555) 123-4567`, `+1 555 123 4567`) are correctly redacted and detected. All four targeted tests pass (`test_us_phone_number_redaction`, `test_us_phone_formats`, `test_detect_phone_pii`, `test_phone_at_start_of_text`), and 24/25 tests in `tests/unit/test_pii_scrubber.py` pass (the one remaining failure, `test_mixed_pii_and_text`, is a pre-existing `street_address` regex issue unrelated to this fix). Also cleaned up two pre-existing `ruff` lint errors in the same file (line-too-long `street_address` regex, unused `pii_type` loop variable) that were blocking the commit's pre-commit hook.

**Next steps:**
Open the PR for review, and confirm `make check` / `make test-unit` results are documented in Check-in 2. No further changes planned to `phone_us` scope per PLAN.md.

**Blockers:**
None for this fix. Unrelated to this issue: `make test-unit` currently has ~49 pre-existing failures across other modules in the repo (not caused by this change).

---

### Check-in 2 (end of week)

**PR link:** [link to your submitted pull request]

**Branch:** [fix/146-PII-scrubber-redact-parenthesized-US-phone-numbers]

**What you built:**
Fixed the `phone_us` regex in `safety/pii_scrubber.py` so `scrub()` and `detect()` correctly handle parenthesized US phone numbers like `(555) 123-4567`. The leading `\b` was replaced with a `(?<!\w)` lookbehind (so the match can start on the opening `(`), and `\s` was added alongside `-`/`.` as a valid separator, so both parenthesized and space-separated formats are matched without regressing the previously supported dashed/dotted formats.

**Tests added or updated:**
No test files were changed — `tests/unit/test_pii_scrubber.py` already contained the relevant coverage (`test_us_phone_number_redaction`, `test_us_phone_formats`, `test_detect_phone_pii`, `test_phone_at_start_of_text`), which previously failed against the parenthesized format and now pass against the fix.

**Self-review confirmation:** [ ] make check passes  [ ] make test-unit passes
- `make check` and `make test-unit` do **not** pass repo-wide — both surface pre-existing failures unrelated to this fix (178 pre-existing `ruff` errors across other modules, ~49 pre-existing unit test failures in unrelated files). Confirmed `safety/pii_scrubber.py` itself has 0 lint errors, and `tests/unit/test_pii_scrubber.py` is 24/25 passing (the 1 failure is a pre-existing, unrelated `street_address` regex issue).

**Draft PR feedback received from:** [name or Slack handle, or "none"]