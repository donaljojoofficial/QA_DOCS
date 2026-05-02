# TC002 - Start Attack Simulation

| Field | Details |
|-------|---------|
| **Test Case ID** | TC002 |
| **Test Case Name** | Start Attack Simulation |
| **Module** | Attack Orchestration |
| **Type** | Manual UI |
| **Priority** | High |
| **Severity if Failed** | Critical |
| **Created By** | Donal Jojo |
| **Created Date** | 2026-05-02 |
| **Last Updated** | 2026-05-02 |

---

## Objective

Verify that an authenticated user can start a new Phase 1 simulation attack and is redirected to the attack detail page with the generated plan/status visible.

---

## Preconditions

- App running at `http://localhost:8000`
- User is logged in with an activated account
- Database migrations completed
- At least one LLM provider API key is configured, or the app has a configured fallback suitable for local simulation
- Simulation executor is available

---

## Test Data

| Field | Value |
|-------|-------|
| Target URL | `http://127.0.0.1:4280/` |
| Executor Type | `simulation` |
| Autonomy Mode | Use default available option |

---

## Test Steps

| Step | Action | Expected Result |
|------|--------|-----------------|
| 1 | Navigate to `http://localhost:8000/start/` | Start attack form loads. Target URL, executor type, and autonomy mode controls are visible. |
| 2 | Enter target URL `http://127.0.0.1:4280/` | Target field accepts the URL without UI errors. |
| 3 | Select `Simulation` executor | Simulation option is selected and no SSH-only fields are required. |
| 4 | Select an autonomy mode | Autonomy option is selected successfully. |
| 5 | Submit the form | User is redirected to `/attack/<pk>/`. |
| 6 | Review the attack detail page | Attack name/details, target, current phase, autonomy status, and plan area are visible. |
| 7 | Confirm Phase 1 behavior | Page indicates simulation mode and does not trigger real network scanning. |

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

- [ ] Simulation mode does not run real exploitation or network scanning
- [ ] Logged-out users cannot access `/start/`
- [ ] Form submission does not expose API keys or environment values
- [ ] Invalid target input is rejected or handled safely

---

## Screenshots to Capture

| What to capture | Suggested filename |
|-----------------|-------------------|
| Empty start form | `tc002_start_form.png` |
| Filled simulation form | `tc002_filled_simulation_form.png` |
| Attack detail after submit | `tc002_attack_detail_created.png` |
| Any validation error | `tc002_validation_error.png` |

---

## Pass Criteria

- Valid simulation form redirects to `/attack/<pk>/`
- Attack detail page shows target, phase, plan/status information
- No server error or stack trace appears
- Simulation mode remains safe and local

## Fail Criteria

- Start form cannot be submitted with valid data
- User is not redirected to the attack detail page
- Attack is created with missing or incorrect target/executor data
- Real scanning/exploitation appears to run during Phase 1 simulation
- Sensitive configuration data is displayed

---

## Result (fill in after execution)

| Field | Value |
|-------|-------|
| **Executed By** | |
| **Execution Date** | |
| **Status** | Pass / Fail / Blocked |
| **Result File** | `results/TC002-start-attack-simulation-result.md` |
| **Defects Found** | |
| **Notes** | |
