# TC008 - Report Generation and Latest Report

| Field | Details |
|-------|---------|
| **Test Case ID** | TC008 |
| **Test Case Name** | Report Generation and Latest Report |
| **Module** | Reporting |
| **Type** | Manual UI / API Verification |
| **Priority** | Medium |
| **Severity if Failed** | Major |
| **Created By** | Donal Jojo |
| **Created Date** | 2026-05-02 |
| **Last Updated** | 2026-05-02 |

---

## Objective

Verify that a report can be generated for an attack run and that the latest report endpoint returns the generated payload.

---

## Preconditions

- App running at `http://localhost:8000`
- User is logged in
- A simulation attack run exists at `/attack/<pk>/`
- Attack run has enough state data to generate a report
- Browser dev tools, Postman, or another HTTP client is available for endpoint verification

---

## Test Data

| Field | Value |
|-------|-------|
| Attack ID | Existing simulation attack ID |
| Generate Report Endpoint | `POST /attack/<pk>/report/generate` |
| Latest Report Endpoint | `GET /attack/<pk>/report/latest` |

---

## Test Steps

| Step | Action | Expected Result |
|------|--------|-----------------|
| 1 | Open `/attack/<pk>/` | Attack detail page loads. |
| 2 | Trigger report generation from UI or send `POST /attack/<pk>/report/generate` with valid CSRF/session | Request succeeds without server error. |
| 3 | Wait for generation to complete if asynchronous | UI or endpoint indicates report is generated. |
| 4 | Open `GET /attack/<pk>/report/latest` | JSON response is returned. |
| 5 | Validate response fields | Response includes status, generated timestamp/value, and report payload. |
| 6 | Review report content | Report includes a meaningful executive summary or attack summary based on attack state. |
| 7 | Generate report again | Latest endpoint returns the newest generated report. |
| 8 | Attempt latest report for invalid attack ID | User receives 404 or controlled error, not a stack trace. |

---

## API Checks

- [ ] Latest report endpoint returns JSON
- [ ] Response includes report status
- [ ] Response includes generated time or equivalent metadata
- [ ] Response includes non-empty payload after generation
- [ ] Invalid attack ID is handled cleanly

---

## Security Checks

- [ ] Logged-out users cannot generate or retrieve reports
- [ ] Users cannot retrieve reports for unauthorized attack runs
- [ ] Report payload does not expose API keys, tokens, or environment secrets
- [ ] Invalid IDs do not expose stack traces

---

## Screenshots / Evidence to Capture

| What to capture | Suggested filename |
|-----------------|-------------------|
| Report generation UI/action | `tc008_report_generate_action.png` |
| Generated report shown in UI | `tc008_report_generated_ui.png` |
| Latest report JSON response | `tc008_latest_report_json.png` |
| Invalid report request handling | `tc008_invalid_report_request.png` |

---

## Pass Criteria

- Report generation completes successfully
- Latest report endpoint returns valid JSON with non-empty payload
- Regeneration updates latest report
- Unauthorized and invalid requests are blocked or handled safely

## Fail Criteria

- Report generation fails for a valid attack
- Latest report endpoint returns empty or malformed data after generation
- Report leaks secrets
- Invalid request causes server error

---

## Result (fill in after execution)

| Field | Value |
|-------|-------|
| **Executed By** | |
| **Execution Date** | |
| **Status** | Pass / Fail / Blocked |
| **Result File** | `results/TC008-report-generation-result.md` |
| **Defects Found** | |
| **Notes** | |
