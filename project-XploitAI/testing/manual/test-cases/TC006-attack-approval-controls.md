# TC006 - Attack Approval and Control Actions

| Field | Details |
|-------|---------|
| **Test Case ID** | TC006 |
| **Test Case Name** | Attack Approval and Control Actions |
| **Module** | Attack Control |
| **Type** | Manual UI |
| **Priority** | High |
| **Severity if Failed** | Critical |
| **Created By** | Donal Jojo |
| **Created Date** | 2026-05-02 |
| **Last Updated** | 2026-05-02 |

---

## Objective

Verify that generated attack plans require human approval where expected and that approve, stop, resume, and retry controls update attack state correctly.

---

## Preconditions

- App running at `http://localhost:8000`
- User is logged in
- A simulation attack has been created and is visible at `/attack/<pk>/`
- The attack has a generated plan or phase ready for approval

---

## Test Data

| Field | Value |
|-------|-------|
| Attack Run | Existing TC002-created simulation run |
| Expected Initial Status | `WAITING_FOR_APPROVAL` or equivalent paused approval status |

---

## Test Steps

| Step | Action | Expected Result |
|------|--------|-----------------|
| 1 | Open `/attack/<pk>/` for a newly created simulation run | Attack detail page shows current phase, generated plan, and approval status. |
| 2 | Review available plan steps | Steps are readable and action names are from the allowed registry. |
| 3 | Click Approve Plan | POST succeeds and attack status changes to `RUNNING` or equivalent. |
| 4 | Refresh attack detail page | Updated status persists. Execution progress or command log area is visible. |
| 5 | Click Stop | Attack status changes to `STOPPED` or equivalent. Stop reason or stopped indicator is shown. |
| 6 | Click Resume | Attack status changes away from stopped state and resumes execution or returns to approval as designed. |
| 7 | If a phase has failed, click Retry Phase | Current failed phase is retried and status/plan updates appropriately. |
| 8 | Attempt direct POST to approve/stop/resume while logged out | Request is rejected or redirected to login. |

---

## UI Checks

- [ ] Approve, Stop, Resume, and Retry controls are visible only when valid for the current state
- [ ] Button clicks provide visible state feedback
- [ ] Plan steps remain readable after refresh
- [ ] Command/progress sections do not overlap or truncate important information

---

## Security and Safety Checks

- [ ] Unauthorized users cannot approve or control attacks
- [ ] Approval cannot execute actions outside the predefined action registry
- [ ] Stop action prevents further execution from continuing silently
- [ ] Control actions do not expose internal exceptions
- [ ] Phase 1 remains simulation-only

---

## Screenshots to Capture

| What to capture | Suggested filename |
|-----------------|-------------------|
| Waiting for approval state | `tc006_waiting_for_approval.png` |
| Plan steps before approval | `tc006_plan_steps.png` |
| Running state after approval | `tc006_running_after_approval.png` |
| Stopped state | `tc006_stopped_state.png` |
| Resumed state | `tc006_resumed_state.png` |

---

## Pass Criteria

- Approval changes attack from waiting to running
- Stop changes attack to stopped
- Resume works according to designed state flow
- Unauthorized control attempts are blocked
- No real exploitation occurs in simulation mode

## Fail Criteria

- Attack executes before required approval
- Controls do not update state
- Logged-out users can control attacks
- Invalid state transitions are allowed
- Stack trace or raw exception is shown

---

## Result (fill in after execution)

| Field | Value |
|-------|-------|
| **Executed By** | |
| **Execution Date** | |
| **Status** | Pass / Fail / Blocked |
| **Result File** | `results/TC006-attack-approval-controls-result.md` |
| **Defects Found** | |
| **Notes** | |
