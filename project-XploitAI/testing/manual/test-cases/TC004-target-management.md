# TC004 - Target Management

| Field | Details |
|-------|---------|
| **Test Case ID** | TC004 |
| **Test Case Name** | Target Management |
| **Module** | Target Management |
| **Type** | Manual UI |
| **Priority** | Medium |
| **Severity if Failed** | Major |
| **Created By** | Donal Jojo |
| **Created Date** | 2026-05-02 |
| **Last Updated** | 2026-05-02 |

---

## Objective

Verify that authenticated users can view and manage target records and that invalid target data is handled safely.

---

## Preconditions

- App running at `http://localhost:8000`
- User is logged in with an activated account
- Target management route `/targets/` is enabled
- Test data can be created and deleted without affecting production data

---

## Test Data

| Field | Valid Value | Invalid Value |
|-------|-------------|---------------|
| Target Name | `Local Simulation Target` | blank |
| Target URL | `http://127.0.0.1:4280/` | `not-a-url` |
| Description | `Manual QA target for Phase 1 simulation` | optional |

---

## Test Steps

| Step | Action | Expected Result |
|------|--------|-----------------|
| 1 | Navigate to `/targets/` | Target management page loads and existing targets are listed. |
| 2 | Start adding a new target | Add/create target form or modal is displayed. |
| 3 | Submit valid target data | Target is created and appears in the list. |
| 4 | Open the created target for edit | Existing target details are populated correctly. |
| 5 | Update the target description | Updated value is saved and displayed. |
| 6 | Attempt to create or update a target with invalid URL `not-a-url` | Validation error is shown. Invalid target is not saved. |
| 7 | Attempt to submit required fields blank | Required-field validation is shown. |
| 8 | Delete the test target | Target is removed from the list after confirmation, if confirmation exists. |

---

## UI Checks

- [ ] Target list is readable and does not overflow
- [ ] Add/edit controls are easy to locate
- [ ] Validation errors are displayed next to the relevant fields
- [ ] Delete action requires clear intent or confirmation
- [ ] List updates after create, edit, and delete actions

---

## Security Checks

- [ ] Logged-out users cannot access `/targets/`
- [ ] Invalid URLs are not accepted silently
- [ ] Form errors do not expose server internals
- [ ] Deleting one target does not delete unrelated attack history

---

## Screenshots to Capture

| What to capture | Suggested filename |
|-----------------|-------------------|
| Target management list | `tc004_targets_list.png` |
| Add target form | `tc004_add_target_form.png` |
| Created target in list | `tc004_target_created.png` |
| Invalid URL validation | `tc004_invalid_url_validation.png` |
| Delete confirmation/result | `tc004_target_deleted.png` |

---

## Pass Criteria

- User can create, edit, view, and delete a valid target
- Invalid target input is rejected with a clear error
- Protected access rules are enforced
- No unrelated data is modified

## Fail Criteria

- Target management page fails to load
- Invalid URLs are saved as valid targets
- Target changes are not persisted
- Delete action removes the wrong record
- Server error or stack trace appears

---

## Result (fill in after execution)

| Field | Value |
|-------|-------|
| **Executed By** | |
| **Execution Date** | |
| **Status** | Pass / Fail / Blocked |
| **Result File** | `results/TC004-target-management-result.md` |
| **Defects Found** | |
| **Notes** | |
