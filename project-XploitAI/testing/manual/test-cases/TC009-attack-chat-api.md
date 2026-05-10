# TC009 - Attack Chat API

| Field | Details |
|-------|---------|
| **Test Case ID** | TC009 |
| **Test Case Name** | Dashboard Assistant |
| **Module** | AI Assistant |
| **Type** | Manual UI |
| **Priority** | Medium |
| **Severity if Failed** | Major |
| **Created By** | Donal Jojo |
| **Created Date** | 2026-05-02 |
| **Last Updated** | 2026-05-02 |

---

## Objective

Verify that authenticated users can ask attack-context questions through the chat API, reset chat context, and receive controlled errors for invalid requests.

---

## Preconditions

- App running at `http://localhost:8000`
- User is logged in
- At least one attack run exists in the current session/context
- CSRF token is available from browser cookies or page source
- LLM provider is configured, or expected provider-unavailable behavior is documented

---

## Test Data

| Field | Value |
|-------|-------|
| Assistant Route | `/assistant/` |
| Valid Message | `Summarize the recent recon phase.` |
| Empty Message | ` ` |

---

## Test Steps

| Step | Action | Expected Result |
|------|--------|-----------------|
| 1 | Navigate to `/assistant/` | Dashboard assistant page loads with chat input controls visible. |
| 2 | Send a valid message through the UI | Assistant responds using attack context or available state data. |
| 3 | Ask a follow-up question | Response indicates conversational memory is maintained. |
| 4 | Send empty message to input | Validation error is shown or submission is disabled. |
| 5 | Log out and navigate to `/assistant/` | User is redirected to `/login/`. |

---

## API Checks

- [ ] Valid assistant request returns expected context
- [ ] Message input handles empty text gracefully
- [ ] Provider-unavailable errors are user-friendly and controlled

---

## Security Checks

- [ ] Assistant endpoint requires authentication
- [ ] CSRF protection is enforced for POST requests
- [ ] Assistant response does not reveal API keys or environment secrets
- [ ] User cannot access another user's private attack context

---

## Screenshots / Evidence to Capture

| What to capture | Suggested filename |
|-----------------|-------------------|
| Assistant UI before message | `tc009_assistant_ui.png` |
| Assistant response | `tc009_assistant_response.png` |
| Empty message validation | `tc009_assistant_empty_validation.png` |

---

## Pass Criteria

- Valid authenticated chat request returns a useful JSON reply
- Reset endpoint succeeds
- Empty, malformed, unauthenticated, and CSRF-missing requests are handled safely
- No secrets or stack traces are exposed

## Fail Criteria

- Valid assistant request fails unexpectedly
- Invalid requests trigger 500 server errors
- Logged-out users can use assistant
- Assistant leaks sensitive configuration

---

## Result (fill in after execution)

| Field | Value |
|-------|-------|
| **Executed By** | |
| **Execution Date** | |
| **Status** | Pass / Fail / Blocked |
| **Result File** | `results/TC009-attack-chat-api-result.md` |
| **Defects Found** | |
| **Notes** | |
