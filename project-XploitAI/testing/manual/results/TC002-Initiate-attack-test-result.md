# Test Execution Report — TC002 Start Attack Initiation

| Field | Details |
|-------|---------|
| **Test Case ID** | TC002 |
| **Test Case Name** | Start Attack Initiation |
| **Module** | Attack Orchestration |
| **Type** | Manual UI Testing |
| **Priority** | High |
| **Severity if Failed** | Critical |
| **Environment** | Local Development |
| **Application URL** | http://localhost:8000/start/ |
| **Browser** | Chrome (latest) |
| **OS** | Windows 11 |
| **Tester** | Donal Jojo |
| **Execution Date** | 2026-05-10 |
| **Status** | ✅ PASS |

---

## Objective

Verify that an authenticated user can initiate an attack or quick test and is redirected to the attack detail page with the generated plan/status visible.

---

## Preconditions

- ✅ App running at `http://localhost:8000`
- ✅ User is logged in with an activated account
- ✅ Database migrations completed
- ✅ At least one LLM provider API key is configured, or the app has a configured fallback suitable for local test initiation
- ✅ A target is registered and an executor (simulation, local, SSH, or daemon) is available

---

## Test Data

| Field | Value |
|-------|-------|
| Target URL | `http://127.0.0.1:4280/` |
| Executor Type | `simulation` |
| Autonomy Mode | Default available option |

---

## Execution Steps & Results

| Step | Action | Expected Result | Actual Result | Status |
|------|--------|-----------------|---------------|--------|
| 1 | Navigate to `/start/` or `/quick-test/start/` | Start attack or quick test form loads | Form loaded correctly with URL, executor, and autonomy controls visible | ✅ Pass |
| 2 | Enter target URL `http://127.0.0.1:4280/` | Target field accepts the URL | URL accepted without UI errors | ✅ Pass |
| 3 | Select executor type | Form updates based on selection (e.g., SSH requires credentials/keys) | Simulation executor selected, dynamic form fields updated correctly without SSH requirements | ✅ Pass |
| 4 | Select an autonomy mode | Option selected successfully | Option selected successfully | ✅ Pass |
| 5 | Submit the form | User is redirected to `/attack/<pk>/` | Successfully submitted and redirected to `attack/1/` | ✅ Pass |
| 6 | Review the attack detail page | Attack details, current phase, autonomy status, and links to phase-reviews/command-logs are visible | Attack details, phase info, status, and associated log links successfully rendered | ✅ Pass |

---

## UI Validation Checks

- ✅ Start form renders without broken layout
- ✅ Required fields are clearly marked
- ✅ Submit button is enabled only when required data is valid
- ✅ Redirect after submit lands on the correct attack detail page
- ✅ Attack status/phase is visible on the detail page
- ✅ User-facing errors are clear if LLM/provider configuration is missing

---

## Security and Safety Checks

- ✅ Logged-out users cannot access `/start/` (redirects to login)
- ✅ Form submission does not expose API keys or environment values
- ✅ Invalid target input is rejected or handled safely

---

## Evidence / Screenshots

| Screenshot | Path |
|------------|------|
| Empty start form | `testing/manual/screenshots/tc002_start_form.png` |
| Filled initiate test form | `testing/manual/screenshots/tc002_filled_simulation_form.png` |
| Attack detail after submit | `testing/manual/screenshots/tc002_attack_detail_created.png` |
| Any validation error | `testing/manual/screenshots/tc002_validation_error.png` |

---

## Observations

- Attack simulation initialization completes seamlessly.
- The system generates the initial state and redirects correctly without any latency or backend stack traces.
- UI properly communicates the transition from setup to the active attack tracking page.

---

## Defects Found

None

---

## Final Result

> **✅ PASS** — The Attack Orchestration module successfully initiates attacks and transitions the user to the attack tracking interface as expected.