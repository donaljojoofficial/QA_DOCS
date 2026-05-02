# TC007 - Attack History and Delete

| Field | Details |
|-------|---------|
| **Test Case ID** | TC007 |
| **Test Case Name** | Attack History and Delete |
| **Module** | Attack History |
| **Type** | Manual UI |
| **Priority** | Medium |
| **Severity if Failed** | Major |
| **Created By** | Donal Jojo |
| **Created Date** | 2026-05-02 |
| **Last Updated** | 2026-05-02 |

---

## Objective

Verify that attack runs appear in history, can be opened from history, and can be deleted without removing unrelated records unexpectedly.

---

## Preconditions

- App running at `http://localhost:8000`
- User is logged in
- At least two simulation attack runs exist for the test user

---

## Test Data

| Item | Value |
|------|-------|
| Run A | Simulation run created during TC002 |
| Run B | Second simulation run created for delete verification |

---

## Test Steps

| Step | Action | Expected Result |
|------|--------|-----------------|
| 1 | Navigate to `/history/` | History page loads and shows existing attack runs. |
| 2 | Locate Run A and Run B | Each run displays enough identifying data such as name, target, status, or date. |
| 3 | Open Run A from history | Browser navigates to `/attack/<pk>/` for Run A. |
| 4 | Return to `/history/` | History page still lists Run A and Run B. |
| 5 | Delete Run B using the run-specific delete action | Run B is removed after confirmation, if confirmation exists. |
| 6 | Confirm Run A still exists | Run A remains visible and can still be opened. |
| 7 | Use `/history/delete-all/` only in a disposable test environment | All attack history for the test scope is removed or confirmation is required before removal. |
| 8 | Refresh `/history/` | Page reflects the updated list without stale deleted entries. |

---

## UI Checks

- [ ] History list is readable with multiple entries
- [ ] Empty history state is understandable
- [ ] Delete actions are visually distinct from open/view actions
- [ ] Page updates after deletion
- [ ] No broken links remain for deleted attacks

---

## Security Checks

- [ ] Logged-out users cannot access `/history/`
- [ ] Delete actions require POST or equivalent protection
- [ ] Deleting one run does not delete unrelated runs
- [ ] User cannot access deleted attack detail by direct URL
- [ ] No stack trace appears for deleted or missing run

---

## Screenshots to Capture

| What to capture | Suggested filename |
|-----------------|-------------------|
| History list with multiple runs | `tc007_history_list.png` |
| Opened attack from history | `tc007_opened_attack_from_history.png` |
| Delete confirmation | `tc007_delete_confirmation.png` |
| History after deletion | `tc007_history_after_delete.png` |
| Empty history state | `tc007_empty_history.png` |

---

## Pass Criteria

- Created attack runs appear in history
- History entries open the correct attack details
- Run-specific delete removes only the selected run
- Delete-all behavior is controlled and visible
- Missing/deleted runs do not crash the app

## Fail Criteria

- History page fails to load
- Entries open the wrong attack
- Deleting one run deletes unrelated data
- Deleted attacks remain accessible
- Delete actions are possible while logged out

---

## Result (fill in after execution)

| Field | Value |
|-------|-------|
| **Executed By** | |
| **Execution Date** | |
| **Status** | Pass / Fail / Blocked |
| **Result File** | `results/TC007-history-and-delete-result.md` |
| **Defects Found** | |
| **Notes** | |
