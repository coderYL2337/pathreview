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

**Reproduction commit link:** [link to commit documenting the reproduced issue]

**Reproduction summary:**
I reproduced issue #146 by running PIIScrubber with both (555) 123-4567 and 555-123-4567. scrub() redacted only the dashed number and detect() returned an empty list for the parenthesized number, confirming the bug.

**PLAN.md link:** [link to PLAN.md in your fork]

**Walkthrough video (recommended):** [link to your Loom video, ≤2 min — shared for early feedback]

**Blockers or open questions:**
[Anything you're still uncertain about going into Week 9, or leave blank]