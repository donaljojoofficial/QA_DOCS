# TC002 - Initiate attack test

| Field | Details |
|-------|---------|
| **Test Case ID** | TC002 |
| **Test Case Name** | Start Attack Initiation |
| **Module** | Attack Orchestration |
| **Type** | Manual UI |
| **Priority** | High |
| **Severity if Failed** | Critical |
| **Created By** | Donal Jojo |
| **Created Date** | 2026-05-10 |
| **Last Updated** | 2026-05-10 |

---

## Objective

Verify that an authenticated user can initiate an attack or test and is redirected to the attack detail page with the generated plan/status visible.

---

## Preconditions

- App running at `http://localhost:8000`
- User is logged in with an activated account
- Database migrations completed
- At least one LLM provider API key is configured, or the app has a    configured fallback suitable for local test initiation
- executor is available

---

## Test Data

| Field | Value |
|-------|-------|
| Target URL | `http://127.0.0.1:4280/` |
| Executor Type | `simulation`, `local`, `ssh`, or `daemon` |
| Autonomy Mode | Use default available option |

---

## Test Steps

| Step | Action | Expected Result |
|------|--------|-----------------|
| 1 | Navigate to `/start/` or `/quick-test/start/` | Start attack or quick test form loads. Target URL, executor type, and autonomy mode controls are visible. |
| 2 | Enter target URL `http://127.0.0.1:4280/` | Target field accepts the URL without UI errors. |
| 3 | Select executor type | Form updates based on selection (e.g., SSH requires credentials/keys). |
| 4 | Select an autonomy mode | Autonomy option is selected successfully. |
| 5 | Submit the form | User is redirected to `/attack/<pk>/`. |
| 6 | Review the attack detail page | Attack details, current phase, autonomy status, and links to phase-reviews/command-logs are visible. |

---

## UI Checks

- [ ] Start form renders without broken layout
- [ ] Required fields are clearly marked
- [ ] Submit button is enabled only when required data is valid
- [ ] Redirect after submit lands on the correct attack detail page
- [ ] Attack status/phase is visible on the detail page
- [ ] User-facing errors are clear if LLM/provider configuration is missing

---

## Security and Safety Checks

- [ ] Logged-out users cannot access `/start/`
- [ ] Form submission does not expose API keys or environment values
- [ ] Invalid target input is rejected or handled safely

---

## Screenshots to Capture

| What to capture | Suggested filename |
|-----------------|-------------------|
| Empty start form | `tc002_start_form.png` |
| Filled initiate test form | `tc002_filled_simulation_form.png` |
| Attack detail after submit | `tc002_attack_detail_created.png` |
| Any validation error | `tc002_validation_error.png` |

---

## Pass Criteria

- Valid initiate test form submits and redirects to `/attack/<pk>/`
- Attack detail page shows target, phase, plan/status information
- No server error or stack trace appears

## Fail Criteria

- Start form cannot be submitted with valid data
- User is not redirected to the attack detail page
- Attack is created with missing or incorrect target/executor data
- Sensitive configuration data is displayed

---

## Result (fill in after execution)

| Field | Value |
|-------|-------|
| **Executed By** | Donal Jojo |
| **Execution Date** | 2026-05-10 |
| **Status** | Pass |
| **Result File** | `results/TC002-Initiate-attack-test-result.md` |
| **Defects Found** | None |
| **Notes** | Executed in Phase 1 simulation mode. All UI validation and redirects worked properly. |
