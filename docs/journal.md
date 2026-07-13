## Week 7 — Issue selection

**Issue link:** [(https://github.com/jamjamgobambam/pathreview/issues/78)]

**Issue title:** [API response for profile creation doesn't include the profile_id field
 #78]

**Tier:** [x] Tier 1  [ ] Tier 2  [ ] Tier 3

**Problem summary:**
When a new profile is created successfully, the API response does not include the profile_id value. Clients can view the response, but they cannot reliably run follow-up actions such as fetch, update, or related requests because those operations require the id. The id exists in the database model, so the gap is in the response schema layer rather than persistence. The fix is to update the profile response schema in api/schemas/profile.py so profile_id is returned after creation and clients can continue their workflow without extra lookup logic.

**Branch name:** [fix/78-add-profile-id-field-in-response-for-profile-creation]

**Setup confirmation:** [x] App runs locally at localhost:5173

**Cohort ledger:** [ ] Issue added to cohort ledger